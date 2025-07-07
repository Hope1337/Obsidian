[[Drawing 2025-07-01 23.25.58.excalidraw]]

Model Context Protocol là một giao thức giúp các công cụ giao tiếp với nhau. MCP hoạt động dựa trên mô hình client-server. Thấy nhiều nhất là ở các ứng dụng dùng AI như một bộ não suy nghĩ, có tích hợp MCP client.

Tức là ngoài việc LLM đóng vai trò như một bộ não, MCP gồm hai thành phần chính là MCP server và MCP client. MCP client thường được tích hợp sẵn trong ứng dụng AI, MCP server là các tool bên ngoài. Trong đó:

==MCP client có trách nhiệm:==
- Xem các tools nào đang hiện có
- Nhận data từ MCP server
- Quản lý và thực thi các tools, bởi vì LLM cũng chỉ là text generator, MCP client mới là người thực thi

==MCP server có trách nhiệm:==
- Cung cấp các tools hoặc services, có 3 dịch vụ chính:
	- **Prompt**: trả về prompt template
	- **Resource**: data, filesystem, database
	- **Tool**: function, API ...
- Lắng nghe và xử lý các request từ MCP client.

==Lưu ý==: LLM chỉ được access vào **tools**, resource và prompt template thì là backend nội bộ.