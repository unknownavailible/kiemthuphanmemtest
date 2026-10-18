# Báo cáo đối chiếu và tiến độ đồ án kiểm thử Mealie

Ngày kiểm tra: 18/10/2026.

## Kết quả đối chiếu

Có hai nguồn cần phân biệt:

- [Repository đồ án được cung cấp](https://github.com/Huuthien12/R02-K05-Mealie-QA): README có đủ 20 mục được yêu cầu. Việc có đề mục không có nghĩa mọi công việc đã hoàn tất hoặc đã được xác minh độc lập.
- Workspace `D:\kiemthuphanmem`: hiện chỉ có thư mục `mealie`, remote là `https://github.com/mealie-recipes/mealie.git`. Đây là mã nguồn SUT; chưa thấy repository QA riêng và tài liệu đồ án theo cấu trúc của đường dẫn trên.

Bảng sau đánh giá hồ sơ local; trạng thái trên GitHub chỉ được đối chiếu ở mức nội dung README.

| STT | Mục cần có | Bằng chứng trong workspace | Đánh giá local |
| --- | --- | --- | --- |
| 1 | Giới thiệu | README upstream mô tả sản phẩm | Cần giới thiệu riêng cho đồ án |
| 2 | Mục tiêu | Chưa thấy mục tiêu kiểm thử của nhóm | Cần bổ sung |
| 3 | System Under Test | Có mã nguồn và commit Git | Có; cần chốt baseline |
| 4 | Công nghệ và công cụ | Có pyproject, frontend và cấu hình Docker | Có nền tảng; thiếu môi trường Schemathesis riêng |
| 5 | Thành viên và phân công | Chưa thấy bảng phân công đồ án | Cần bổ sung |
| 6 | Kiến trúc kiểm thử | Có tests upstream và fixtures | Cần mô tả kiến trúc QA riêng |
| 7 | Cấu trúc repository | Có cấu trúc mã nguồn Mealie | Cần cấu trúc hồ sơ QA |
| 8 | Trạng thái dự án | Có source; chưa kiểm tra runtime | Cần sổ theo dõi tiến độ |
| 9 | Phạm vi kiểm thử | Chưa thấy tài liệu scope đồ án | Cần bổ sung |
| 10 | Chiến lược kiểm thử | Có bộ test của upstream | Cần chiến lược đồ án |
| 11 | Business Flows | Có test recipe, shopping list và meal plan | Có tài liệu tham khảo từ code; cần đặc tả luồng |
| 12 | Cài đặt và chạy Mealie | Có docker/docker-compose.yml | Có cấu hình; chưa xác minh chạy thành công |
| 13 | API Documentation | Có backend FastAPI | Cần xác minh URL tài liệu trên runtime |
| 14 | OpenAPI Snapshot | Chưa thấy snapshot QA được lưu riêng | Cần thu thập |
| 15 | Schemathesis | Không tìm thấy trong tests và pyproject đã kiểm tra | Cần thiết lập |
| 16 | Git Workflow | Có Git và workflows upstream | Cần quy trình nhóm |
| 17 | Security & Safety | Có SECURITY.md upstream | Cần quy tắc an toàn cho campaign QA |
| 18 | Tài liệu dự án | Có tài liệu sản phẩm và phát triển | Thiếu hồ sơ đồ án riêng |
| 19 | Tiến độ tiếp theo | Chưa thấy kế hoạch đồ án | Cần lập kế hoạch |
| 20 | Ghi chú | README upstream | Cần ghi giả định và giới hạn kiểm tra |

Nội dung bên dưới là bản đề xuất cho hồ sơ local, không xác nhận nhóm đã thực hiện các phần chưa có bằng chứng.

## 1. Giới thiệu

Đồ án nghiên cứu kiểm thử REST API của Mealie, hệ thống quản lý công thức nấu ăn, kế hoạch bữa ăn và danh sách mua sắm. Hướng triển khai đề xuất là kiểm thử dựa trên OpenAPI kết hợp sinh dữ liệu với Schemathesis và kiểm thử nghiệp vụ có chủ đích.

Mã nguồn Mealie là đối tượng kiểm thử. Bộ test do upstream cung cấp là nguồn tham khảo; nhóm cần chỉ rõ phần phân tích, thiết kế và tự động hóa do mình thực hiện.

## 2. Mục tiêu

- Thiết lập môi trường kiểm thử có thể tái lập và ghi lại phiên bản chính xác.
- Thu thập OpenAPI, chọn phạm vi và xây dựng yêu cầu có mã định danh.
- Thiết kế test case, expected result và ma trận truy vết.
- Chạy kiểm thử, lưu kết quả thực tế, phân loại failure và lập hồ sơ lỗi.
- Tổng hợp giới hạn, mức bao phủ và kết luận dựa trên bằng chứng.

## 3. System Under Test

| Thuộc tính | Giá trị đã đọc từ local |
| --- | --- |
| Sản phẩm | Mealie |
| Thư mục | D:\kiemthuphanmem\mealie |
| Remote | https://github.com/mealie-recipes/mealie.git |
| HEAD | 879b133b5f98dd91419948f363d2d6d58793c3a0 |
| git describe | v3.28.0-82-g879b133b5 |
| Version trong pyproject.toml | 3.28.0 |
| Backend | Python / FastAPI / SQLAlchemy |
| CSDL trong Compose hiện có | SQLite |
| Ánh xạ cổng trong Compose | 9091:9000 |

HEAD local không trùng commit baseline trong README QA được tham chiếu. Không dùng version string 3.28.0 để kết luận hai bản mã nguồn giống nhau. Nhóm cần chọn commit đích và ghi cả image ID/digest nếu chạy container.

## 4. Công nghệ và công cụ

| Công cụ | Vai trò | Trạng thái xác minh |
| --- | --- | --- |
| Python | Backend và test upstream | pyproject yêu cầu >=3.14,<3.15; chưa kiểm tra bản cài trên máy |
| FastAPI, Pydantic | REST API và mô hình dữ liệu | Có khai báo trong mã nguồn |
| SQLAlchemy, SQLite/PostgreSQL | Lưu trữ dữ liệu | Có hỗ trợ trong dự án |
| Docker Compose | Chạy SUT | Có file cấu hình; chưa chạy kiểm tra |
| pytest | Test backend | Có dependency và bộ test upstream |
| Playwright | Test E2E | Có trong tests/e2e/package.json |
| Schemathesis | Sinh test dựa trên schema | Đề xuất bổ sung cho QA |
| Git | Quản lý baseline và thay đổi | Đã đọc được commit và remote |

Python của SUT và Python của bộ QA có thể khác nhau nếu QA chạy qua HTTP trong môi trường riêng. Cần khóa dependency sau khi kiểm tra tương thích; không sao chép phiên bản công cụ từ repository khác một cách mặc định.

## 5. Thành viên và phân công

Chưa có dữ liệu xác nhận thành viên của workspace này. Điền bảng sau trước khi nộp:

| Thành viên / MSSV | Trách nhiệm đề xuất | Đầu ra |
| --- | --- | --- |
| Chưa cung cấp | Điều phối và quản lý lỗi | Kế hoạch, defect log, báo cáo |
| Chưa cung cấp | Môi trường và baseline | Setup guide, version manifest, snapshot |
| Chưa cung cấp | Phân tích và thiết kế | Đặc tả, scope, test cases, ma trận |
| Chưa cung cấp | Tự động hóa và kết quả | Test harness, logs, JUnit, hướng dẫn chạy |

Đây là phân công mẫu; nhóm cần điều chỉnh theo số thành viên thực tế.

## 6. Kiến trúc kiểm thử

```text
Mealie commit đã chốt + OpenAPI snapshot
                  |
        Yêu cầu và expected result
                  |
       Test cases và ma trận truy vết
                  |
     pytest + Schemathesis + token QA
                  |
          Mealie API trong lab
                  |
       HTTP response + trạng thái dữ liệu
                  |
       Đối chiếu -> kết quả -> hồ sơ lỗi
```

Phân biệt kiểm tra hợp đồng OpenAPI với quy tắc nghiệp vụ của nhóm. Các test tạo dữ liệu cần quản lý ID và cleanup trong cơ chế teardown/finally.

## 7. Cấu trúc repository

Cấu trúc thực tế hiện thấy: workspace chứa `mealie/` với `mealie/`, `frontend/`, `tests/`, `docker/` và `docs/` bên trong repository upstream.

Đề xuất tạo repository QA riêng, cạnh thư mục SUT:

```text
mealie-qa/                         # đề xuất, chưa tạo
  README.md
  setup/                          # hướng dẫn, manifest phiên bản
  schema/snapshots/                # OpenAPI và checksum
  docs/analysis/
  docs/specification/
  docs/test-design/               # test cases và traceability
  docs/defects/
  tests/                          # harness của nhóm
  reports/                        # kết quả từng lần chạy
  evidence/                       # request, response và minh chứng
```

## 8. Trạng thái dự án

| Hạng mục | Trạng thái | Điều kiện xác nhận hoàn thành |
| --- | --- | --- |
| Có mã nguồn SUT | Đã xác minh | Có remote và commit |
| Có cấu hình triển khai | Đã xác minh | Có Compose và ánh xạ cổng |
| SUT hoạt động | Chưa xác minh | Container healthy và HTTP response |
| Bộ test upstream | Đã xác minh tồn tại | Chưa xác minh kết quả chạy |
| Snapshot QA | Chưa thấy | Lưu schema, metadata và hash |
| Đặc tả/test design của nhóm | Chưa thấy | Có review và phiên bản |
| Schemathesis QA | Chưa thấy | Có môi trường khóa phiên bản và test harness |
| Kết quả, defect và báo cáo | Chưa thấy hồ sơ QA | Có evidence truy vết được |

Không đưa tỷ lệ hoàn thành hoặc số PASS/FAIL của repository tham khảo vào kết quả local khi chưa có lần chạy tương ứng.

## 9. Phạm vi kiểm thử

Phạm vi đề xuất gồm Recipes, Shopping Lists, Shopping Items và Meal Plans. Chốt danh sách method/path sau khi có snapshot của đúng baseline.

- P0: kiểm tra đọc dữ liệu, phân trang, lọc, định danh và xác thực trên các GET được chọn.
- P1: tạo, đọc, cập nhật nếu được API hỗ trợ, xóa và kiểm tra trạng thái của tài nguyên QA.
- Boundary: thiếu trường, null, rỗng, sai kiểu và giá trị biên theo schema từng operation.

Chưa đưa quản trị hệ thống, backup/restore, import từ dịch vụ ngoài và dữ liệu thật vào campaign ban đầu.

## 10. Chiến lược kiểm thử

1. Chốt commit, cấu hình, tài khoản và schema trước khi chạy.
2. Gán mã REQ cho yêu cầu; mô tả căn cứ của expected result.
3. Ánh xạ REQ -> TC -> operation -> assertion.
4. Chạy smoke/P0 trước, sau đó chạy P1 có kiểm soát.
5. Lưu thời gian, phiên bản, dữ liệu đầu vào, actual và kết quả.
6. Phân loại failure: lỗi SUT, sai lệch schema, lỗi môi trường hoặc lỗi harness.
7. Tái hiện tối thiểu và chạy regression sau thay đổi liên quan.

Không mặc định mọi dữ liệu không hợp lệ phải trả cùng một mã HTTP. PASS của một test không chứng minh toàn bộ API đúng.

## 11. Business Flows

| Luồng đề xuất | Các bước | Kỳ vọng cần đặc tả |
| --- | --- | --- |
| Recipe | Tạo -> đọc -> cập nhật nếu hỗ trợ -> xóa | Dữ liệu đọc nhất quán; xử lý sau xóa theo contract/quy tắc đã chốt |
| Shopping List | Tạo -> đọc -> đổi tên -> đọc lại -> xóa | Tên và định danh được lưu đúng |
| Shopping Item | Tạo list cha -> tạo item -> cập nhật -> xóa item -> xóa list | Item thuộc đúng list; cleanup đúng thứ tự |
| Meal Plan | Tạo -> đọc -> cập nhật -> xóa | Ngày, loại bữa ăn và tham chiếu đúng theo schema |

Mỗi luồng cần tiền điều kiện, payload, expected HTTP/body và hậu điều kiện. Các test upstream hiện có về recipe CRUD, shopping list/items và meal plan có thể dùng để nghiên cứu.

## 12. Cài đặt và chạy Mealie

Tại local có `mealie/docker/docker-compose.yml`, image `mealie:dev`, build target `production`, volume `mealie-data` và port `9091:9000`.

Các lệnh dưới đây là hướng dẫn đề xuất, chưa được thực thi trong lần kiểm tra tài liệu này:

```powershell
Set-Location D:\kiemthuphanmem\mealie
git rev-parse HEAD
docker compose -f docker/docker-compose.yml config --quiet
docker compose -f docker/docker-compose.yml up -d --build
docker compose -f docker/docker-compose.yml ps
```

Ghi lại baseline được nhóm chấp thuận trước khi build. Xác minh health và log khởi động trước khi chạy test. Không xóa volume đang chứa dữ liệu cần giữ.

## 13. API Documentation

Sau khi SUT khởi động, kiểm tra các địa chỉ dự kiến và ghi lại kết quả thực tế:

| Tài nguyên | URL cần xác minh |
| --- | --- |
| UI | http://localhost:9091 |
| Swagger UI | http://localhost:9091/docs |
| ReDoc | http://localhost:9091/redoc |
| OpenAPI | http://localhost:9091/openapi.json |
| Thông tin ứng dụng | http://localhost:9091/api/app/about |

Các URL này là địa chỉ kiểm tra dự kiến, chưa được xác nhận trả HTTP 200 trong đợt rà soát.

## 14. OpenAPI Snapshot

Lưu snapshot từ đúng runtime đã chốt vào repository QA. Ví dụ khi đang đứng tại thư mục gốc QA:

```powershell
New-Item -ItemType Directory -Force schema/snapshots
Invoke-WebRequest -Uri http://localhost:9091/openapi.json -OutFile schema/snapshots/mealie-openapi.json
Get-FileHash schema/snapshots/mealie-openapi.json -Algorithm SHA256
```

Lưu kèm commit SUT, image ID/digest, thời gian lấy và URL nguồn. Đếm paths/operations trực tiếp từ snapshot; chưa gán số liệu của repository tham khảo cho local.

## 15. Schemathesis

Chưa thấy cấu hình Schemathesis trong các vị trí local đã kiểm tra. Công việc cần làm:

- Tạo môi trường QA riêng và khóa phiên bản Python/dependency tương thích.
- Nạp snapshot và chọn operation bằng allowlist method/path.
- Đọc token từ biến môi trường; che token trong log.
- Giới hạn dữ liệu, thời gian và số ví dụ; ghi seed khi công cụ hỗ trợ.
- Tách test đọc và test ghi; đảm bảo cleanup cho test ghi.
- Xuất JUnit và lưu request/response phục vụ triage.

Chỉ sử dụng lệnh chạy cụ thể sau khi đã triển khai harness và kiểm tra cú pháp với phiên bản Schemathesis được chọn.

## 16. Git Workflow

Đề xuất dùng nhánh `docs/*`, `setup/*`, `test/*` và `report/*`, review qua Pull Request trước khi merge. Mỗi lần chạy cần liên kết với commit của QA và commit/image SUT.

Giữ thay đổi của bộ QA trong repository riêng để thể hiện rõ đóng góp của nhóm. Không thay baseline hoặc chỉnh expected result chỉ để loại bỏ FAIL.

## 17. Security & Safety

Chỉ chạy campaign trên local/lab được cho phép. Không commit token, mật khẩu hoặc file cấu hình bí mật; `.env.example` chỉ dùng placeholder. Token và thông tin nhạy cảm phải được che trước khi lưu evidence.

Test ghi chỉ thao tác dữ liệu QA do test quản lý, dùng tên duy nhất, lưu ID và cleanup. Không chạy fuzz không giới hạn trên dữ liệu thật hay môi trường công khai.

## 18. Tài liệu dự án

| Tài liệu cần hoàn thiện | Nội dung tối thiểu |
| --- | --- |
| Setup guide | Điều kiện cài đặt, build/run và xử lý lỗi |
| Environment manifest | Commit, image, công cụ và cấu hình |
| System/API analysis | Chức năng, quyền và danh sách operation |
| Functional specification | REQ, input, expected và nguồn căn cứ |
| Test scope/design | Phạm vi, luồng, boundary và oracle |
| Traceability matrix | REQ, TC, actual, evidence và defect |
| Test report | Ngày chạy, baseline, PASS/FAIL/SKIP và giới hạn |
| Defect log | Tái hiện, expected/actual, mức độ và evidence |
| Reproducibility guide | Các bước chạy lại từ môi trường sạch |

File báo cáo này cung cấp khung tổng hợp; các hồ sơ chi tiết vẫn cần được nhóm xây dựng và kiểm chứng.

## 19. Tiến độ tiếp theo

| Thứ tự | Công việc | Tiêu chí bàn giao |
| --- | --- | --- |
| 1 | Chốt repository QA, thành viên và baseline | Có người phụ trách, commit và phạm vi thống nhất |
| 2 | Khởi động Mealie | Có health, HTTP response và log |
| 3 | Thu thập snapshot | Có schema, metadata và SHA256 |
| 4 | Phân tích và thiết kế test | Có REQ/TC/expected và review |
| 5 | Triển khai P0 | Có harness, authentication và báo cáo lần chạy |
| 6 | Triển khai P1 | Có luồng dữ liệu QA và bằng chứng cleanup |
| 7 | Phân tích failure | Có reproducer, phân loại và defect log |
| 8 | Chạy lại và hoàn thiện báo cáo | Có ma trận cập nhật, kết quả và kịch bản demo |

Chưa có deadline và người phụ trách thực tế nên chưa gán ngày hoàn thành hoặc phần trăm tiến độ.

## 20. Ghi chú

- Đã kiểm tra file, cấu hình và metadata Git; chưa khởi động Docker hoặc chạy test.
- Không chỉnh mã nguồn, checkout commit khác, cài dependency hay thay đổi dữ liệu ứng dụng trong đợt kiểm tra này.
- Có đủ 20 mục trong README GitHub được cung cấp; workspace local chưa có bộ hồ sơ tương đương.
- Các bước tương lai trong báo cáo là đề xuất, không phải bằng chứng đã hoàn thành.
- Nguồn local chính: mealie/README.md, mealie/AGENTS.md, mealie/pyproject.toml, mealie/docker/docker-compose.yml, mealie/tests/ và mealie/tests/e2e/package.json.
