---
trigger: always_on
---

# Oxil AI Engine Tools Guidelines

Dự án này được tích hợp MCP Server **Oxil AI Engine** (`oxil-ai-engine`) qua giao thức SSE (`https://oxil.tringuyencse.com/sse`).

## Danh Mục Công Cụ & Hướng Dẫn Sử Dụng

Khi làm việc trong codebase này, hãy ưu tiên sử dụng các công cụ chuyên dụng sau:

### 1. Tra cứu & Phân tích Ngữ cảnh
* `hybrid_context_search`: Tra cứu ngữ cảnh 3 lớp (Vector Qdrant + Neo4j Graph + BM25) khi cần tìm kiếm toàn diện.
* `qdrant_vector_search`: Tìm kiếm tương đồng ngữ nghĩa (semantic) trực tiếp trên vector codebase.
* `sync_codebase_graph`: Đồng bộ cấu trúc Nodes & Edges (quan hệ giữa các file, caller/callee) vào Neo4j Graph.
* `task_blast_radius`: Phân tích phạm vi ảnh hưởng khi sửa đổi/refactor một file hoặc module (Caller/Callee tree).

### 2. Kiểm soát chất lượng & Kiểm tra PRD
* `reasoning_gate_scan`: Quét tài liệu PRD hoặc đề xuất kỹ thuật qua 7 tiêu chí rủi ro (idempotency, auth, rate limiting, error handling,...).
* `clarify_scan_markers`: Quét tự động các đánh dấu `[NEEDS CLARIFICATION]` còn tồn đọng trong tài liệu.
* `multi_agent_seed`: Tổng hợp ngữ cảnh từ Jira, Git và Neo4j thành context map gọn nhẹ (≤4000 tokens) cho agent.

### 3. Tích hợp Quản lý Dự án (Jira / Confluence)
* `jira_get_issue`: Lấy thông tin chi tiết ticket Jira (Status, Summary, Description, Comments).
* `jira_create_issue`: Tạo task/bug/story mới trên Jira board.
* `confluence_publish_doc`: Xuất bản tài liệu kiến trúc, PRD hoặc hướng dẫn lên Confluence Space.

### 4. Vòng đời Task & Tự động hoá
* `task_start`: Khởi tạo Dossier cho task/project mới.
* `task_generate_prd`: Tự động sinh hồ sơ PRD, câu hỏi làm rõ và email trao đổi.
* `task_generate_architecture`: Thiết kế Architecture Blueprint & DB Schema tương ứng.
* `task_generate_code`: Sinh mã nguồn và checkout branch git theo quy chuẩn.
* `task_handover`: Tổng hợp tài liệu bàn giao và đóng task.
