# The AI Engineer Evolution: From Prompt to Agentic Graph & Production Harness

Tài liệu chuyên sâu và slide trình bày về kiến trúc, nguyên lý và bản đồ công nghệ của **AI Engineer / Agentic AI**: từ Prompt Engineering, RAG, MCP (Model Context Protocol) đến Graph Engineering và Production Harness.

---

## 📂 Cấu trúc dự án

```text
.
├── docs/
│   ├── Graph_Engineer.md   # Kiến trúc Graph Engineering & Workflow đa tác nhân
│   ├── MCP.md              # Chuẩn giao tiếp Model Context Protocol
│   ├── RAG.md              # Retrieval-Augmented Generation nâng cao
│   └── Skill.md            # Skill Engineering & Function Calling
├── slides/
│   ├── slides.md           # Slide deck tương tác viết bằng Slidev
│   ├── style.css           # Custom styling cho slide
│   ├── package.json        # Dependencies & scripts cho Slidev
│   └── ai_engineer_evolution.pdf # File PDF xuất bản sẵn của slide deck
└── source/
    └── Graph-Engineering-Andrew-Ng-Playbook.pdf # Tài liệu tham khảo gốc
```

---

## 🚀 Hướng dẫn chạy Slidev

Thư mục `slides/` sử dụng [Slidev](https://sli.dev/) để tạo bài thuyết trình dành cho developer.

### Yêu cầu
- Node.js >= 18.0.0

### Khởi chạy môi trường phát triển
```bash
cd slides
npm install
npm run dev
```

### Build và Export
- **Build trang web tĩnh (SPA):**
  ```bash
  npm run build
  ```
- **Export ra PDF:**
  ```bash
  npm run export
  ```
- **Export từng slide ra định dạng PNG:**
  ```bash
  npm run export-png
  ```

---

## 📚 Nội dung chính trong Docs

1. **[Graph Engineering](docs/Graph_Engineer.md)**: Xây dựng workflow phức tạp, State Machine, chu trình lặp (Feedback Loops), Human-in-the-loop và tự sửa lỗi (Self-reflection).
2. **[MCP (Model Context Protocol)](docs/MCP.md)**: Kiến trúc kết nối mở giữa LLM và các hệ thống dữ liệu / công cụ bên ngoài theo tiêu chuẩn mở của Anthropic.
3. **[RAG (Retrieval-Augmented Generation)](docs/RAG.md)**: Chiến lược phân tách (chunking), embedding, hybrid search, reranking và truy vấn đa nguồn.
4. **[Skill Engineering](docs/Skill.md)**: Đóng gói năng lực tác vụ, function calling, tool use và guardrails.
