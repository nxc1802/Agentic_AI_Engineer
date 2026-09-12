Trong **AI Engineer hiện đại**, đặc biệt với kiến trúc **Agentic AI**, *Skill* có thể hiểu là một **năng lực chuyên biệt mà AI Agent có thể kích hoạt để hoàn thành một loại nhiệm vụ cụ thể**.

Nó nằm giữa **LLM** và **Tool/Workflow**:

> **LLM = bộ não** → **Skill = năng lực** → **Tool = công cụ thực thi**

### 1. Skill là gì?

Một Skill không nhất thiết là một model mới. Nó thường là một **gói instruction + knowledge + tools + workflow + rules** được thiết kế để Agent biết cách xử lý một nhóm nhiệm vụ.

Ví dụ một AI Engineer Agent có:

| Skill                   | Nhiệm vụ                          |
| ----------------------- | --------------------------------- |
| `Python Skill`          | Viết, debug và chạy Python        |
| `Research Skill`        | Tìm kiếm, đọc và tổng hợp paper   |
| `Data Analysis Skill`   | Phân tích CSV/Excel, tạo biểu đồ  |
| `GitHub Skill`          | Đọc repo, issue, PR, chỉnh code   |
| `SQL Skill`             | Viết và kiểm tra SQL              |
| `RAG Skill`             | Xây dựng và truy vấn hệ thống RAG |
| `Computer Vision Skill` | Xử lý ảnh, OCR, detection         |
| `Math Skill`            | Giải toán + sử dụng calculator    |
| `Manim Skill`           | Sinh và sửa animation             |

Điểm quan trọng là **Skill không chỉ là prompt**.

Một Skill tốt có thể chứa:

```text
Skill
├── Instructions
├── Domain Knowledge
├── Tools
├── Workflow
├── Constraints
├── Examples
├── Validation
└── Error Recovery
```

---

## 2. Skill khác Tool như thế nào?

Đây là điểm rất quan trọng khi thiết kế AI Engineer.

Ví dụ:

```text
Skill: Data Analysis
        │
        ├── pandas
        ├── Python execution
        ├── visualization
        └── statistical analysis
```

Ở đây:

* **Python** → Tool
* **pandas** → Tool/Library
* **Data Analysis** → Skill

Skill quyết định **khi nào và như thế nào sử dụng các tool**.

Ví dụ user hỏi:

> "Phân tích file CSV này và tìm các outlier."

Agent có thể:

```text
User
 ↓
LLM
 ↓
Data Analysis Skill
 ↓
1. Load CSV
2. Inspect schema
3. Clean data
4. Calculate statistics
5. Detect outliers
6. Visualize
7. Validate result
 ↓
Final Answer
```

Nếu chỉ có Tool:

```text
LLM → Python
```

LLM phải tự quyết định toàn bộ workflow.

Nếu có Skill:

```text
LLM
 ↓
Data Analysis Skill
 ↓
Python + pandas + visualization
```

Agent có **quy trình chuyên môn ổn định hơn**.

---

# 3. Skill khác Agent như thế nào?

Có thể hình dung:

```text
                 AI Engineer
                     │
             ┌───────┴───────┐
             │     Agent     │
             └───────┬───────┘
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
    Skill A      Skill B      Skill C
    Coding       Research     Data Analysis
        │            │            │
      Tools        Tools        Tools
```

**Agent** là thực thể điều phối.

**Skill** là khả năng chuyên môn.

Ví dụ:

> "Đọc paper này, lấy dataset, viết code preprocessing và chạy benchmark."

Agent có thể kết hợp:

```text
Research Skill
      ↓
Coding Skill
      ↓
Data Analysis Skill
      ↓
Reporting Skill
```

Vì vậy Skill giúp chuyển Agent từ:

> **general-purpose chatbot**

thành:

> **general-purpose AI worker có nhiều chuyên môn**

---

# 4. Skill trong quá trình tiến hóa của AI Engineer

Nếu nhìn AI Engineer theo các giai đoạn phát triển, Skill xuất hiện khá tự nhiên:

```text
LLM Engineer
     ↓
RAG Engineer
     ↓
Tool Engineer
     ↓
Agent Engineer
     ↓
Skill-based Agent Engineer
     ↓
AI System / AI Engineer
```

### Giai đoạn đầu

LLM chỉ:

```text
Prompt → LLM → Answer
```

### Thêm RAG

```text
Prompt
  ↓
LLM + Knowledge
  ↓
Answer
```

### Thêm Tool

```text
LLM
 ↓
Tool selection
 ↓
Tool execution
 ↓
Answer
```

### Thêm Agent

```text
Goal
 ↓
Planning
 ↓
Tool selection
 ↓
Execution
 ↓
Observation
 ↓
Reasoning
 ↓
Result
```

### Thêm Skill

Agent bắt đầu có **các năng lực chuyên môn reusable**:

```text
                    Agent
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
  Coding Skill   Research Skill   Math Skill
       │              │              │
    Tools           Tools           Tools
```

Đây là bước rất quan trọng vì hệ thống bắt đầu có tính **modular và composable**.

---

# 5. Một Skill tốt nên có cấu trúc như thế nào?

Ví dụ `Research Skill`:

```text
research/
├── SKILL.md
├── instructions/
│   ├── search.md
│   ├── paper_analysis.md
│   └── citation.md
│
├── tools/
│   ├── web_search
│   └── pdf_reader
│
├── knowledge/
│   └── research_guidelines.md
│
└── workflows/
    └── literature_review.md
```

`SKILL.md` có thể định nghĩa:

```text
# Research Skill

## Purpose
Conduct systematic literature research.

## Workflow
1. Understand research question
2. Search literature
3. Filter relevant papers
4. Read papers
5. Extract methodology
6. Compare related work
7. Identify research gap
8. Produce cited synthesis

## Rules
- Prefer primary sources
- Verify publication venue
- Do not fabricate citations
- Distinguish facts from inference

## Tools
- Web Search
- PDF Reader
- Citation Manager
```

Như vậy Skill giống một **mini operating manual cho Agent**.

---

# 6. Skill có thể được kích hoạt động

Đây là một điểm mạnh của kiến trúc Skill.

Không cần nhồi tất cả instructions vào system prompt.

Ví dụ Agent có:

```text
Skills:
├── coding
├── research
├── computer_use
├── data_analysis
├── mathematics
├── writing
└── github
```

User hỏi:

> "Review PR này và đề xuất cách sửa."

Agent nhận diện:

```text
github skill
+
coding skill
```

User hỏi:

> "Tìm các paper về 3D geometric reasoning của LLM."

Agent dùng:

```text
research skill
```

User hỏi:

> "Tải dataset rồi train model."

Agent có thể kết hợp:

```text
research/data skill
+
coding skill
+
experiment skill
```

Đây chính là **Skill Composition**.

---

# 7. Skill Composition

Một hệ thống AI Engineer mạnh không nhất thiết cần hàng trăm Agent.

Thay vào đó có thể có:

```text
                    AI Engineer Agent
                           │
              Skill Router / Planner
                           │
       ┌───────────┬───────┼────────┬──────────┐
       ↓           ↓       ↓        ↓          ↓
   Research     Coding    GitHub   Data      Writing
       │           │        │       │           │
       └───────────┴────────┴───────┴───────────┘
                           │
                         Tools
```

Ví dụ workflow nghiên cứu:

```text
Research Skill
      ↓
GitHub Skill
      ↓
Coding Skill
      ↓
Experiment Skill
      ↓
Data Analysis Skill
      ↓
Writing Skill
```

Một **AI Engineer thực sự** vì vậy không chỉ biết xây Agent, mà còn biết **thiết kế, tổ chức và phối hợp các Skill**.

---

# 8. Skill, MCP và Tool

Ba khái niệm này cũng rất dễ bị nhầm:

| Thành phần   | Vai trò                                         |
| ------------ | ----------------------------------------------- |
| **Model**    | Reasoning / generation                          |
| **Skill**    | Cách thực hiện một loại nhiệm vụ                |
| **Tool**     | Khả năng thực thi hành động                     |
| **MCP**      | Chuẩn/kênh kết nối model/agent với tools & data |
| **Agent**    | Điều phối reasoning + skill + tool              |
| **Workflow** | Chuỗi bước thực thi được định nghĩa trước       |

Có thể hình dung:

```text
                 Agent
                   │
             ┌─────┴─────┐
             │  Skill    │
             └─────┬─────┘
                   │
          ┌────────┼────────┐
          ↓        ↓        ↓
       Tool A   Tool B    Tool C
          │        │        │
          └────────┼────────┘
                   ↓
              Environment
```

**MCP không phải Skill.**

MCP chủ yếu giải quyết bài toán **kết nối và chuẩn hóa access tới tools/resources**, còn Skill giải quyết **AI nên sử dụng những capability đó như thế nào để hoàn thành nhiệm vụ**.

---

# 9. Skill là một trong những nền tảng của AI Engineer hiện đại

Nếu phải cô đọng thành một kiến trúc:

```text
                       AI Engineer
                            │
        ┌───────────────────┼───────────────────┐
        ↓                   ↓                   ↓
      Model              Skills              Memory
        │                   │                   │
   Reasoning         ┌──────┼──────┐       Context
                     ↓      ↓      ↓
                  Tools  RAG   Workflow
                     │      │      │
                     └──────┼──────┘
                            ↓
                         Agent
                            ↓
                       Environment
```

Vì vậy, **Skill là lớp biến một LLM/Agent tổng quát thành một AI Engineer có năng lực chuyên môn có thể tái sử dụng**.

Nếu chia tiến trình học AI Engineer thành các stage, tôi sẽ đặt **Skill sau Tool/Agent và trước Multi-Agent/AI System Engineering**: đây là bước chuyển từ *"biết gọi tool"* sang *"biết đóng gói năng lực để Agent có thể sử dụng một cách có hệ thống"*.
