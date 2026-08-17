# 🚀 Oxil AI Engine - MCP Server Integration Guide

> **Official Endpoint:** `https://oxil.tringuyencse.com/sse`  
> **Protocol:** Model Context Protocol (MCP) over Server-Sent Events (SSE)  
> **Security:** Bearer Token / API Key Authentication

---

## 🔑 1. Thông Tin Kết Nối & Bảo Mật

* **Server URL:** `https://oxil.tringuyencse.com/sse` (hoặc `http://213.35.96.102:8080/sse` khi test trực tiếp IP)
* **Auth Header:** `Authorization: Bearer oxil_sec_9e7f82a1b4c3d5e6f708192a3b4c5d6e7f8a9b0c`

---

## 💻 2. Hướng Dẫn Cài Đặt Vào IDE

### A. Cấu hình cho Cursor IDE
1. Mở Cursor, vào **Settings** ➔ **Features** ➔ **MCP Servers** (hoặc mở file `~/.cursor/mcp.json`).
2. Thêm cấu hình MCP Server:
```json
{
  "mcpServers": {
    "oxil-ai-engine": {
      "url": "https://oxil.tringuyencse.com/sse",
      "headers": {
        "Authorization": "Bearer oxil_sec_9e7f82a1b4c3d5e6f708192a3b4c5d6e7f8a9b0c"
      }
    }
  }
}
```

---

### B. Cấu hình cho Claude Desktop
1. Mở file cấu hình Claude Desktop:
   * **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
   * **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
2. Thêm cấu hình:
```json
{
  "mcpServers": {
    "oxil-ai-engine": {
      "url": "https://oxil.tringuyencse.com/sse",
      "headers": {
        "Authorization": "Bearer oxil_sec_9e7f82a1b4c3d5e6f708192a3b4c5d6e7f8a9b0c"
      }
    }
  }
}
```

---

### C. Cấu hình cho Antigravity IDE
Trong workspace của bạn, tạo file `.agents/mcp_config.json` (hoặc mở `~/.gemini/config/mcp_config.json`):
```json
{
  "mcpServers": {
    "oxil-ai-engine": {
      "$typeName": "exa.cascade_plugins_pb.CascadePluginCommandTemplate",
      "command": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3",
      "args": [
        "/Users/tringuyencse/Projects/AI/oxil-ai-engine/app/mcp_server.py",
        "--stdio"
      ],
      "env": {
        "PYTHONPATH": "/Users/tringuyencse/Projects/AI/oxil-ai-engine"
      }
    }
  }
}
```

---

## 🛠️ 3. Danh Mục Công Cụ (Tool Catalog - DLC V3)

| Tên Tool | Mô tả | Công nghệ kết nối |
|---|---|---|
| `hybrid_context_search` | Tìm kiếm ngữ cảnh 3 lớp (Vector + Neo4j Graph + BM25) | Qdrant Cloud + Neo4j AuraDB |
| `qdrant_vector_search` | Tra cứu semantic trực tiếp trên vector codebase | Qdrant Cloud (`oxil_source_context`) |
| `task_blast_radius` | Phân tích phạm vi ảnh hưởng khi sửa file (Caller/Callee) | Neo4j AuraDB Call Graph |
| `sync_codebase_graph` | Đồng bộ Nodes & Edges (Quan hệ giữa các file) vào Graph | AST Parser + Neo4j AuraDB |
| `reasoning_gate_scan` | Quét tài liệu PRD qua 7 rủi ro kỹ thuật (idempotency, auth, etc.) | Rule-based & LLM Gate |
| `clarify_scan_markers` | Quét các nhãn `[NEEDS CLARIFICATION]` trong tài liệu | Clarification Scanner |
| `multi_agent_seed` | Gom context từ Jira, Git và Neo4j thành context map ≤4000 tokens | Multi-source aggregator |
| `jira_get_issue` | Lấy chi tiết ticket Jira (Status, Summary, Description) | Jira Cloud REST API (`THN`) |
| `jira_create_issue` | Tạo task mới trên Jira board | Jira Cloud REST API (`THN`) |
| `confluence_publish_doc` | Xuất bản tài liệu kỹ thuật lên Confluence Space | Confluence Cloud REST API |
| `task_start` | Khởi tạo Dossier cho task/project mới | Local Task System |
| `task_generate_prd` | Sinh hồ sơ PRD, bộ câu hỏi và Email | BA Generator |
| `task_generate_architecture`| Thiết kế Architecture Blueprint & DB Schema | Architecture Engine |
| `task_generate_code` | Sinh mã nguồn và tự động checkout branch | Git & Code Generator |
| `task_handover` | Tổng hợp tài liệu bàn giao và đóng task | Handover Pipeline |

---

## 🌐 4. Hướng Dẫn Cấu Hình Cloudflare DNS

Để kích hoạt domain `oxil.tringuyencse.com` có SSL miễn phí:
1. Đăng nhập vào trang quản trị **Cloudflare Dashboard** ➔ Chọn domain `tringuyencse.com`.
2. Vào mục **DNS** ➔ **Records** ➔ Click **Add record**:
   * **Type:** `A`
   * **Name:** `oxil`
   * **IPv4 address:** `213.35.96.102`
   * **Proxy status:** `Proxied` (Bật đám mây cam 🟠)
   * **TTL:** `Auto`
3. Nhấn **Save**. Sau 1 phút, domain `https://oxil.tringuyencse.com/sse` sẽ sẵn sàng hoạt động với HTTPS.

---

## 🔍 5. Kiểm Tra Hoạt Động (Health Check)

Kiểm tra từ Terminal bằng curl:
```bash
# 1. Health check (Không cần key)
curl https://oxil.tringuyencse.com/health

# 2. Test kết nối SSE có kèm Bearer Token
curl -N -H "Authorization: Bearer oxil_sec_9e7f82a1b4c3d5e6f708192a3b4c5d6e7f8a9b0c" \
     https://oxil.tringuyencse.com/sse
```
