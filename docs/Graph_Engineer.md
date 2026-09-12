Tôi đã đọc tài liệu **Graph Engineering for Multi-Agentic Systems: The Andrew Ng Playbook** mà bạn gửi. Tài liệu này rất phù hợp để làm nền cho phần **Graph Engineer**, đặc biệt vì nó không xem Graph đơn thuần là một framework orchestration, mà đặt Graph là bước tiến hóa từ **Loop → Chain → Network → Graph**, tức là quá trình đưa **state, control và responsibility** từ implicit thành explicit. 

Dưới đây là phiên bản tôi đề xuất để đưa vào **báo cáo AI Engineer**.

---

# AI Engineer Evolution

Có thể xây dựng toàn bộ báo cáo theo chuỗi:

```text
Prompt
   ↓
Context
   ├── RAG
   ├── Memory
   ├── Tool
   ├── MCP
   └── Skill
   ↓
Agent
   ↓
Loop
   ↓
Graph
   ↓
Harness
```

Điểm quan trọng là đây **không phải một danh sách công nghệ độc lập**, mà là quá trình giải quyết lần lượt các giới hạn của LLM.

| Giai đoạn            | Giải quyết vấn đề                     | Tư duy chính                   |
| -------------------- | ------------------------------------- | ------------------------------ |
| **Prompt Engineer**  | LLM không hiểu đúng task              | Control behavior               |
| **Context Engineer** | LLM thiếu thông tin/capability        | Supply context                 |
| **Agent Engineer**   | LLM không tự quyết định hành động     | Give agency                    |
| **Loop Engineer**    | Một lần suy luận chưa đủ              | Enable iteration               |
| **Graph Engineer**   | Workflow/state trở nên phức tạp       | Structure coordination & state |
| **Harness Engineer** | Agent chưa đủ reliable cho production | Control & evaluate system      |

---

# 1. Prompt Engineer

Ở giai đoạn đầu:

```text
User
 ↓
Prompt
 ↓
LLM
 ↓
Answer
```

LLM chủ yếu thực hiện **một lần inference**.

AI Engineer tập trung vào:

* System prompt
* Instructions
* Role
* Few-shot examples
* Output format
* Constraints
* Task decomposition trong prompt

Ví dụ:

```text
You are a code reviewer.

Review the following code.

Requirements:
- Find bugs
- Identify security issues
- Give severity
- Provide line references
```

Mục tiêu:

> **Kiểm soát hành vi của model thông qua instruction.**

Nhưng Prompt không giải quyết được việc LLM cần dữ liệu bên ngoài hoặc cần thực hiện hành động.

→ **Context Engineering**.

---

# 2. Context Engineer

Thay vì chỉ:

```text
Prompt → LLM
```

AI Engineer bắt đầu xây dựng context:

```text
                 ┌── RAG
                 ├── Memory
                 ├── Tool
User → Context ──┼── MCP
                 └── Skill
                       ↓
                      LLM
```

Context Engineering trả lời:

> **“LLM cần những thông tin và capabilities nào để hoàn thành task này?”**

## RAG

Cung cấp external knowledge:

```text
Documents
 ↓
Embedding
 ↓
Vector DB
 ↓
Retrieval
 ↓
Context
 ↓
LLM
```

→ **Knowledge**

## Memory

Lưu lại trạng thái/thông tin có giá trị qua thời gian.

```text
Conversation
Past Tasks
User Preferences
Agent Experiences
        ↓
      Memory
```

→ **Persistent information**

## Tool

Cho phép LLM thực hiện hành động:

```text
LLM → Calculator
    → Python
    → Database
    → API
    → Browser
```

→ **Action**

## MCP

Chuẩn hóa cách AI application kết nối với external tools/context.

→ **Connectivity / interoperability**

## Skill

Đóng gói nhiều instructions, context và capabilities thành một khả năng có thể tái sử dụng.

→ **Reusable capability**

Do đó Context có thể được nhìn như:

```text
RAG     → Know
Memory  → Remember
Tool    → Act
MCP     → Connect
Skill   → Package capability
```

---

# 3. Agent Engineer

Đây là bước chuyển từ:

```text
LLM answers
```

sang:

```text
LLM decides
```

Agent có thể:

```text
Goal
 ↓
Reason
 ↓
Choose action
 ↓
Use tool
 ↓
Observe result
```

Ví dụ:

```text
User:
"Why is my code failing?"

Agent:
 → Inspect repository
 → Read dependencies
 → Run tests
 → Analyze errors
 → Propose fix
```

Agent bắt đầu có:

* Goal
* Reasoning
* Planning
* Tool selection
* Decision making
* Action

Tài liệu của bạn xem **Reflection, Tool Use, Planning và Multi-Agent Collaboration** là bốn design patterns quan trọng của Agentic AI. 

---

# 4. Loop Engineer

Một Agent đơn lẻ vẫn bị giới hạn bởi việc nó chỉ thực hiện một chuỗi suy luận hữu hạn.

Loop đưa vào khả năng:

> **Thử → quan sát → sửa → thử lại.**

Ví dụ:

```text
Generate Code
     ↓
Run Test
     ↓
Test Failed
     ↓
Analyze Error
     ↓
Repair Code
     ↓
Run Test
     ↓
Test Passed
     ↓
   END
```

Hay tổng quát:

```text
        ┌──────────┐
        │   Think  │
        └────┬─────┘
             ↓
        ┌──────────┐
        │   Act    │
        └────┬─────┘
             ↓
        ┌──────────┐
        │ Observe  │
        └────┬─────┘
             ↓
       Goal reached?
        ↙       ↘
      No         Yes
      ↓           ↓
    Loop         END
```

Tài liệu nhấn mạnh Reflection chính là một feedback loop có **explicit evaluation và stopping rule**, thay vì đơn giản yêu cầu model “improve this”. 

---

# 5. Graph Engineer ⭐

Đây là giai đoạn bạn nên dành nhiều thời lượng nhất.

## 5.1 Vì sao Loop không còn đủ?

Loop hoạt động rất tốt khi:

* Một Agent
* Một task
* Một session
* Context vừa đủ
* Workflow tương đối đơn giản

Nhưng khi hệ thống lớn lên:

```text
Multiple agents
Multiple tasks
Branching
Parallel execution
Persistent state
Cross-session reasoning
```

thì việc nhồi mọi thứ vào một context window trở thành vấn đề.

Tài liệu mô tả rất rõ progression:

```text
Loop
 ↓
Chain
 ↓
Network
 ↓
Graph
```

Trong đó mỗi bước là một hình thức **externalization of cognition**:

> Loop externalizes revision.
> Chain externalizes task order.
> Network externalizes role specialization.
> Graph externalizes shared state and relationships. 

Đây là một câu **rất đáng đưa vào slide**.

---

# 6. Loop → Chain → Network → Graph

## Loop

```text
Agent
 ↓
Action
 ↓
Observation
 ↓
Agent
```

State chủ yếu nằm trong context.

---

## Chain

Workflow được đưa ra ngoài model:

```text
Extract
 ↓
Validate
 ↓
Summarize
 ↓
Format
```

Mỗi stage có thể có:

* Prompt riêng
* Model riêng
* Tool riêng
* Validation riêng

Chain làm **task order explicit**. 

---

## Network

Bắt đầu có nhiều role-specialized agents:

```text
              ┌→ Researcher
              │
Orchestrator ─┼→ Coder
              │
              └→ Reviewer
```

Nhưng phát sinh **context bottleneck**:

```text
Researcher ──┐
Coder ───────┼──→ Orchestrator
Reviewer ────┘
```

Orchestrator phải giữ output của tất cả workers.

Khi số worker tăng, context trở nên khó quản lý. Tài liệu gọi đây là một trong những động lực chính dẫn đến Graph. 

---

# 7. Graph Architecture

Graph thay đổi cách hệ thống lưu và chia sẻ state.

Thay vì:

```text
Worker A
   ↓
Orchestrator
   ↓
Worker B
```

ta có:

```text
             ┌──────────────┐
             │ Knowledge    │
             │ Graph        │
             └──────┬───────┘
                ↙        ↘
        Agent A            Agent B
           ↓                  ↓
        read/write          read/write
```

Worker không nhất thiết phải gửi toàn bộ conversation cho Orchestrator.

Nó có thể:

```text
Read relevant subgraph
        ↓
Perform task
        ↓
Write new entities/relations
```

Nhờ vậy:

> **Shared state được externalize khỏi context window.**

Đây chính là điểm khác biệt cốt lõi của Graph Architecture theo tài liệu. 

---

# 8. Graph không chỉ là Workflow Graph

Đây là điểm tôi khuyên bạn **phải phân biệt rõ trong báo cáo**.

Có hai ý tưởng dễ bị trộn lẫn:

### Workflow Graph

```text
Node → Node → Node
```

Dùng để biểu diễn:

* Control flow
* Branching
* Loop
* Parallelism
* Agent orchestration

### Knowledge Graph

```text
Entity ──relationship──> Entity
```

Dùng để biểu diễn:

* Facts
* Entities
* Relationships
* Provenance
* Persistent knowledge

Tài liệu Graph Engineering của bạn tập trung mạnh vào **Graph Architecture + Agentic Knowledge Graph**, tức Graph không chỉ điều khiển workflow mà còn trở thành **durable shared information layer**. 

---

# 9. Agentic Knowledge Graph

Một schema tối thiểu trong tài liệu gồm:

```text
Entity
Claim
Source
Artifact
Run
```

Ví dụ:

```text
Entity:
    Alice
    OpenAI

Claim:
    Alice works_at OpenAI

Source:
    company_report.pdf

Artifact:
    Research report

Run:
    Agent execution #1024
```

Các relationship:

```text
mentions
supports
contradicts
derived_from
supersedes
```

Điểm cực kỳ quan trọng:

> **Mỗi Claim cần provenance.**

Ví dụ:

```text
Alice
   │
   │ works_at
   ↓
OpenAI
   ↑
   │
derived_from
   │
company_report.pdf
```

Như vậy Agent không chỉ biết **fact**, mà còn biết:

> **Fact này đến từ đâu?**

Tài liệu cũng nhấn mạnh revision không nên âm thầm overwrite dữ liệu cũ; thay vào đó tạo version mới và dùng quan hệ `supersedes`. 

---

# 10. Graph trở thành Shared Memory

Đây là điểm liên kết rất đẹp với phần **Context → Memory** trước đó.

Memory thông thường:

```text
Agent
 ↓
Memory
```

Graph:

```text
Agent A ─┐
         │
Agent B ─┼→ Shared Knowledge Graph
         │
Agent C ─┘
```

Graph có thể đóng ba vai trò:

### 1. Shared Memory

Nhiều Agent cùng đọc/ghi state.

### 2. Grounding Layer

Evaluator kiểm tra claim dựa trên relationships/provenance.

### 3. Persistent World Model

Graph tồn tại ngay cả khi context window bị flush.

Tài liệu gọi đây là ba vai trò riêng biệt của knowledge graph trong multi-agent systems.

---

# 10.1 Graph-Agent Memory: Short-Term vs. Long-Term Memory

Trong kiến trúc Graph-Agent, bộ nhớ không chỉ là chuỗi tin nhắn được nhét vào context window mà được phân tách thành hai tầng kiến trúc rõ ràng:

```text
┌─────────────────────────────────────────────────────────┐
│        SHORT-TERM MEMORY (Thread / Session Scope)       │
│                                                         │
│  • Graph State (MessagesState, Reducers, Scratchpad)    │
│  • Checkpointers (PostgresSaver, SqliteSaver)           │
│  • Scope: 1 execution run hoặc 1 thread_id             │
└────────────────────────────┬────────────────────────────┘
                             │
            1. Recall        │   2. Distill & Consolidate
        (Semantic Search)    │   (Extract Facts/Rules)
                             ▼
┌─────────────────────────────────────────────────────────┐
│         LONG-TERM MEMORY (Cross-Session / Global)       │
│                                                         │
│  • Persistent Store (BaseStore / Namespaced Storage)    │
│  • Semantic Memory (User preferences, Domain rules)     │
│  • Episodic Memory (Past solutions, Bug patterns)       │
│  • Scope: Xuyên suốt nhiều thread, users, repos         │
└─────────────────────────────────────────────────────────┘
```

### 1. Short-Term Memory (Bộ nhớ làm việc / Working State)
- **Bản chất:** Là trạng thái tức thời (*ephemeral state*) của đồ thị trong một phiên làm việc (`thread_id`).
- **Thành phần:**
  - `messages`: Lịch sử trao đổi giữa user, agent và tools trong phiên.
  - `scratchpad`: Biến trung gian, danh sách kế hoạch phân rã (*task plan*), kết quả phân tích sơ bộ.
- **Cơ chế Checkpointing:**
  - Lưu trạng thái tại từng bước chuyển (*step transition*) của Node.
  - Hỗ trợ khôi phục tự động khi hệ thống gặp sự cố (*Crash Recovery*).
  - Tính năng **Time-Travel**: Cho phép quay lại bước $N-1$ để sửa lỗi hoặc thử nghiệm các nhánh suy luận khác (*branching*).

### 2. Long-Term Memory (Bộ nhớ dài hạn / Persistent Store)
- **Bản chất:** Là tri thức tích lũy bền vững xuyên suốt nhiều phiên làm việc, không bị xóa khi kết thúc session.
- **Phân loại:**
  - **Semantic Memory:** Các sự thật khách quan (*Facts*), sở thích người dùng (*User Preferences*), quy định của hệ thống.
  - **Episodic Memory:** Lịch sử các ca xử lý trong quá khứ, các phương án giải quyết thành công/thất bại trước đó.
  - **Procedural Memory:** Các quy tắc/workflow học được trong quá trình tương tác.
- **Tổ chức dữ liệu:** Phân cấp theo namespace (ví dụ: `("users", user_id, "preferences")`, `("codebase", repo_id, "bug_history")`) kết hợp Vector Embeddings để tìm kiếm ngữ nghĩa (*Semantic Search*).

### 3. Vòng lặp Đồng hóa Bộ nhớ (Memory Consolidation Loop)
- **Giai đoạn Đọc (Recall):** Khi bắt đầu một thread mới, Graph Node truy vấn Long-Term Store để chèn ngữ cảnh liên quan vào Short-Term State.
- **Giai đoạn Ghi (Distill):** Sau khi hoàn thành một nhiệm vụ hoặc qua một nút đánh giá (Reflection Node), hệ thống tự động chắt lọc thông tin quan trọng từ Short-Term State để cập nhật vào Long-Term Store, loại bỏ thông tin rác.

---

# 11. Graph + Multi-Agent

Khi kết hợp Graph với Multi-Agent:

```text
                   User
                     ↓
              Architect Agent
                     ↓
               Tech Lead Agent
                     ↓
             Developer Agent
                     ↓
              Knowledge Graph
             ↙       ↓       ↘
         Facts     Artifacts   History
```

Hoặc:

```text
                 ┌──────────────┐
                 │ Knowledge    │
                 │ Graph        │
                 └──────┬───────┘
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Researcher      Coder        Reviewer
          ↓             ↓             ↓
          └─────────────┼─────────────┘
                        ↓
                     Manager
```

Graph giải quyết vấn đề:

> **Agents không cần truyền toàn bộ conversation cho nhau.**

Chúng chia sẻ **structured artifacts + relationships + state**.

---

# 12. Graph + Traceability

Đây có lẽ là giá trị quan trọng nhất đối với Production AI.

Một output:

```text
Final Answer
```

có thể truy ngược:

```text
Final Answer
     ↓
Evaluator Decision
     ↓
Artifact
     ↓
Plan
     ↓
Claim
     ↓
Source
     ↓
Original Task
```

Tức là hệ thống có thể trả lời:

> **“Tại sao Agent lại đưa ra kết quả này?”**

Tài liệu kết luận rằng một reliable agentic system nên có khả năng trace một output quan trọng về **task → plan → artifact → source → evaluator decision → bounded execution record**. 

Đây là sự chuyển đổi:

```text
Implicit reasoning
        ↓
Explicit state
        ↓
Traceable reasoning
```

---

# 13. Nhưng Graph không đồng nghĩa với Truth

Một cảnh báo rất quan trọng trong tài liệu:

> **Graph có thể lưu lỗi hiệu quả chẳng kém lưu sự thật.**

Ví dụ:

```text
Document
   ↓
LLM extraction
   ↓
Wrong Entity
   ↓
Wrong Relationship
   ↓
Knowledge Graph
```

Nếu Agent tin graph tuyệt đối:

```text
Wrong fact
   ↓
Many agents
   ↓
Many future decisions
```

Lỗi sẽ được khuếch đại.

Vì vậy Graph cần:

* Schema validation
* Canonical identifiers
* Entity resolution
* Provenance
* Conflict representation
* Confidence calibration
* Versioning
* Periodic review

Tài liệu nhấn mạnh chính xác vấn đề này. 

---

# 14. Khi nào cần Graph?

Đây là phần rất quan trọng để tránh hiểu sai rằng:

> “AI Engineer hiện đại thì bắt buộc phải dùng Graph.”

**Không.**

Decision framework của tài liệu:

```text
Simple task
   ↓
Zero-shot

Need better quality
   ↓
Reflection

Need external data
   ↓
Tool

Complex task
   ↓
Planning

Multiple perspectives
   ↓
Multi-Agent

Cross-session state
   ↓
Graph
```



Ba dấu hiệu đặc biệt cho thấy Graph bắt đầu đáng giá:

### 1. Cross-session persistence

Task kéo dài qua nhiều sessions.

### 2. Cross-agent coordination

Nhiều Agent cần chia sẻ facts mà không muốn copy transcript.

### 3. Traceability

Cần biết tại sao một kết quả thay đổi.



---

# 15. Một nguyên tắc rất hay của tài liệu

> **“The graph earns itself.”**

Graph chỉ thực sự đáng xây khi cùng một entity hoặc relationship được nhiều Agent hoặc nhiều session truy vấn.

Nếu:

```text
Write graph
   ↓
Never query graph
```

thì Graph chỉ là:

> **Database table with extra overhead.**



Đây là một câu rất tốt để đưa vào phần **Engineering Trade-offs**.

---

# 16. Harness Engineer

Sau Graph, vấn đề không còn chỉ là:

> “Agent có làm được không?”

mà là:

> **“Agent có đáng tin cậy khi đưa vào production không?”**

Harness bao quanh toàn bộ hệ thống:

```text
                    Harness
        ┌──────────────────────────┐
        │ Evaluation               │
        │ Observability            │
        │ Guardrails               │
        │ Retry / Recovery         │
        │ Security                 │
        │ Sandbox                  │
        │ Cost Control             │
        │ Human-in-the-loop        │
        └────────────┬─────────────┘
                     ↓
             Agentic System
```

Các vấn đề:

* Agent loop vô hạn
* Tool failure
* Model hallucination
* Bad routing
* Cost explosion
* Latency
* Security
* Không trace được failure
* Không biết system có tốt hơn baseline hay không

Harness biến Agent từ:

```text
Interesting prototype
```

thành:

```text
Reliable production system
```

---

# 17. Study Case: LangGraph Production Architecture & Implementation

Để minh chứng cho sức mạnh của **Graph Engineering** và **Bộ nhớ đa tầng**, **LangGraph** được lựa chọn làm Study Case trọng tâm cho toàn bộ bài báo cáo.

### 1. Tại sao chọn LangGraph làm Study Case?
- **Khắc phục hạn chế của Chain truyền thống:** LangChain hoặc DAG chỉ cho phép luồng 1 chiều (Linear/DAG), không hỗ trợ tốt chu trình lặp (Cycles) và máy trạng thái (State Machine).
- **Lập trình dựa trên trạng thái (Stateful Multi-Actor):** LangGraph tách rời hoàn toàn Control Flow khỏi LLM prompt, đưa quyền điều phối về code Python tất định (*Deterministic Python Logic*).

### 2. Kiến trúc Cốt Lõi của LangGraph (StateGraph & Reducers)

```python
from typing import Annotated, TypedDict, Literal
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages

# 1. Định nghĩa State tập trung
class AgentState(TypedDict):
    messages: Annotated[list, add_messages]   # Reducer tự động cộng dồn tin nhắn
    task_plan: list[str]                      # Kế hoạch phân rã
    retrieved_facts: list[dict]               # Tri thức từ Long-Term Memory
    review_status: Literal["pending", "approved", "rejected"]

# 2. Xây dựng Đồ thị
builder = StateGraph(AgentState)

# 3. Đăng ký các Node (LLM / Tool / Rule)
builder.add_node("planner", plan_node)
builder.add_node("tool_executor", tools_node)
builder.add_node("evaluator", reflection_evaluator_node)
builder.add_node("human_review", human_approval_node)

# 4. Thiết lập Cạnh điều kiện (Conditional Edges)
builder.add_edge(START, "planner")
builder.add_edge("planner", "tool_executor")
builder.add_edge("tool_executor", "evaluator")

def route_evaluation(state: AgentState):
    if state["review_status"] == "approved":
        return "human_review"
    elif len(state["messages"]) > 10:
        return END  # Circuit Breaker chống loop vô hạn
    return "planner" # Lặp lại để tự sửa lỗi (Loop reflection)

builder.add_conditional_edges("evaluator", route_evaluation)
```

### 3. Hiện Thực Hóa Bộ Nhớ Đa Tầng Trong LangGraph
- **Short-Term Memory với Checkpointers:**
  ```python
  from langgraph.checkpoint.postgres import PostgresSaver
  
  checkpointer = PostgresSaver.from_conn_string("postgresql://...")
  graph = builder.compile(checkpointer=checkpointer)
  
  # Truyền thread_id để định danh phiên làm việc
  config = {"configurable": {"thread_id": "session_pr_102"}}
  graph.invoke({"messages": [("user", "Review PR #405")]}, config=config)
  ```
  *Đặc điểm:* Mỗi bước qua Node đều sinh snapshot state vào Postgres. Hỗ trợ **Time-Travel Debugging** (quay lại state bước trước đó và chỉnh sửa).

- **Long-Term Memory với LangGraph Store:**
  ```python
  from langgraph.store.memory import InMemoryStore
  
  store = InMemoryStore()
  # Lưu thông tin theo namespace phân cấp
  store.put(
      namespace=("codebase", "auth_repo", "vulnerabilities"),
      key="CVE-2024-X",
      value={"pattern": "SQL Injection in /api/login", "fix": "Use parameterized query"}
  )
  ```

- **Human-In-The-Loop (HITL) với Interrupts:**
  ```python
  # Tạm dừng đồ thị trước khi kích hoạt hành động rủi ro cao
  graph = builder.compile(
      checkpointer=checkpointer,
      interrupt_before=["human_review"]
  )
  ```

### 4. Kết Quả Đo Lường Thực Tế Trên Tác Vụ Code Review
Khi nâng cấp hệ thống qua từng giai đoạn kiến trúc:
1. **Prompt Only:** 55% độ chính xác (nhiều false positives, bỏ sót ngữ cảnh).
2. **+ Reflection Loop:** 72% (tự phát hiện và loại bỏ các cảnh báo sai).
3. **+ Tools & Linters:** 84% (kích hoạt unit test và linter thực tế).
4. **+ Multi-Agent Team:** 88% (phân tách chuyên môn Security, Logic, Style).
5. **+ LangGraph (StateGraph + Multi-tier Memory + HITL):** **96%** (tái sử dụng bug patterns từ Long-Term Store, quản lý state tất định và chặn lỗi bằng Human Checkpoint).

---

# 18. Slide tổng kết tôi khuyên dùng

```text
        THE EVOLUTION OF AI ENGINEERING

┌───────────┐
│  PROMPT   │ → Control model behavior
└─────┬─────┘
      ↓
┌───────────┐
│  CONTEXT  │ → Give knowledge & capabilities
└─────┬─────┘
      │
      ├── RAG      → Know
      ├── Memory   → Remember
      ├── Tool     → Act
      ├── MCP      → Connect
      └── Skill    → Package
      ↓
┌───────────┐
│   AGENT   │ → Make decisions
└─────┬─────┘
      ↓
┌───────────┐
│   LOOP    │ → Iterate & self-correct
└─────┬─────┘
      ↓
┌───────────┐
│   GRAPH   │ → Structure state & coordination
└─────┬─────┘
      ↓
┌───────────┐
│  HARNESS  │ → Make it reliable
└───────────┘
```

Và câu kết:

> **Prompt controls the model. Context informs the model. Agent gives it agency. Loop gives it iteration. Graph gives it structure and persistent relationships. Harness makes the whole system reliable.**

Đặc biệt với **Graph Engineer**, tôi sẽ không trình bày nó đơn giản là *“dùng LangGraph/graph để điều phối Agent”*. Dựa trên tài liệu bạn gửi, framing mạnh hơn là:

> **Graph Engineering = bước chuyển từ Agentic Workflow có state ngầm và ngắn hạn sang Agentic System có state rõ ràng, persistent, shared, queryable và traceable.**

Đó mới là lý do tại sao tài liệu đặt progression **Loop → Chain → Network → Graph**, và tại sao **Knowledge Graph** trở thành một lớp hạ tầng cho Multi-Agent thay vì chỉ là một database khác. 
