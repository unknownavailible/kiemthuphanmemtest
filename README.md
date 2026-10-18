# Báo cáo đối chiếu và tiến độ đồ án kiểm thử Mealie — R02 + K10

Ngày đối chiếu: 18/10/2026. Số liệu lịch sử trong Word được chốt ngày 16/10/2026.

## Kết quả đối chiếu

Nguồn chính: `Bao_cao_quy_trinh_kiem_thu_Mealie_R02_K10.docx` do người dùng cung cấp. Đề tài đúng là **Visual Regression Testing bằng Playwright**. Repository R02 + K05 được gửi ban đầu chỉ là khung tham khảo 20 mục; không dùng công cụ, thành viên hoặc kết quả nhóm K05 làm dữ liệu của nhóm này. Báo cáo Mẫu thiết kế/Mặc Studio gửi nhầm không được sử dụng.

Bản Markdown cũ được lập trước khi có Word đúng nên đề xuất Schemathesis và để trống thành viên. Bản này thay các nội dung đó bằng dữ liệu K10. Commit SUT local trùng Word; nhận xét trước đây về khác baseline chỉ đúng khi so với repo K05.

Quy ước: **Local** = đã đọc trực tiếp; **Theo Word** = tài liệu ghi nhận, chưa kiểm tra lại artifact; **Đề xuất** = thiết kế chưa thực thi; **Chờ xác nhận** = thiếu bằng chứng hoặc quyết định nhóm.

Workspace hiện có source upstream Mealie, chưa thấy harness `mealie-nhom-23`, ảnh baseline hoặc evidence của nhóm. Không suy việc chưa có file local thành nhóm chưa thực hiện công việc đã mô tả trong Word.

| Mục theo khung ban đầu | Word đúng có chưa? | Điều chỉnh / bổ sung |
| --- | --- | --- |
| 1. Giới thiệu | Có, mục 1 | Chốt R02 + K10 |
| 2. Mục tiêu | Có, mục 1 và 3 | Thêm điều kiện kiểm chứng |
| 3. SUT | Có, mục 2 và 6 | Tách commit/source/runtime |
| 4. Công cụ | Có, mục 6 | Ghi phiên bản theo nguồn |
| 5. Thành viên | Có, mục 4 | Thay bảng trống bằng phân công thật |
| 6. Kiến trúc | Có, mục 2 | Gắn auth/seed/oracle/evidence |
| 7. Cấu trúc repo | Rải ở mục 20 | Tổng hợp danh mục |
| 8. Trạng thái | Có, mục 14 và 19 | Tách đạt kỹ thuật và chờ nghiệm thu |
| 9. Phạm vi | Có, mục 3 | 1 auth + 8 visual + 3 smoke |
| 10. Chiến lược | Có, mục 5, 7 và 16 | Thêm manifest và tiêu chí đóng |
| 11. Business Flows | Có, mục 10 | Giữ đúng ba FLOW |
| 12. Cài đặt | Có, mục 6 và 20 | Tách dòng lệnh; phân biệt máy QA/máy mới |
| 13. API Documentation | Có API trong FLOW, chưa có mục riêng | Thêm URL cần xác minh |
| 14. OpenAPI Snapshot | Chưa thấy snapshot riêng | Tùy chọn hỗ trợ, không bắt buộc K10 |
| 15. Schemathesis | Không thuộc đề tài K10 | Thay bằng Playwright và baseline |
| 16. Git Workflow | Có pin/CI; thiếu quy trình nhánh/review rõ ràng | Bổ sung quy trình thay đổi |
| 17. Security & Safety | Đã có secrets/dữ liệu QA | Bổ sung auth state/artifact |
| 18. Tài liệu dự án | Có, mục 20 | Thêm biểu mẫu và checklist bàn giao |
| 19. Tiến độ | Có, mục 18–19 | Gắn đầu ra và chủ trì đề xuất |
| 20. Ghi chú | Có quy ước/giới hạn | Tổng hợp điểm còn mở |

## 1. Giới thiệu

Đồ án môn Kiểm thử phần mềm, Trường Đại học Đà Lạt, Khoa Công nghệ thông tin; giảng viên Nguyễn Thế Lâm. Tổ hợp R02 + K10 dùng Mealie làm SUT và Playwright để kiểm thử hồi quy giao diện.

Bộ chính bảo vệ tám trạng thái màn hình và ba đường đi API ghi → UI đọc. Kiểm tra route, nội dung và lỗi API/JavaScript phải đi cùng ảnh để tránh so khớp một màn hình không đúng nghiệp vụ.

## 2. Mục tiêu

- Tái lập môi trường, dữ liệu, phiên đăng nhập và điều kiện chụp.
- Bảo vệ tám trạng thái UI bằng ảnh tham chiếu có review.
- Kiểm tra ba luồng dữ liệu cùng cleanup theo ID/slug.
- Phân biệt intentional change, regression, lỗi môi trường và lỗi harness.
- Truy vết kết quả đến source, harness, baseline và lần chạy.

Theo Word, bộ chính đạt 12/12 và hai demo bắt mismatch đúng dự kiến. Chấp nhận baseline và nghiệm thu là các quyết định riêng, chưa thể suy từ pass rate.

## 3. System Under Test

| Thuộc tính | Giá trị | Nguồn |
| --- | --- | --- |
| SUT | Mealie, version source 3.28.0 | Local và Word |
| SUT commit | 879b133b5f98dd91419948f363d2d6d58793c3a0 | Local trùng Word |
| git describe local | v3.28.0-82-g879b133b5 | Rà soát local trước |
| Harness commit | eccf48e65f6fd554759fea79267f1feb7b011a4d | Theo Word |
| Backend QA | Linux container; Python 3.14; host 19000 → 9000 | Theo Word |
| Frontend QA | Nuxt dev trên host, cổng 3000 | Theo Word |
| DB QA | sqlite:////source/dev/data/mealie.db | Theo Word |

Compose upstream local dùng `9091:9000`; cấu hình QA trong Word dùng `19000:9000` và UI tách ở `3000`. Không trộn hai cấu hình. Chưa xác minh runtime, image digest hoặc DB thực trong lượt bổ sung này.

## 4. Công nghệ và công cụ

| Thành phần | Cấu hình ghi trong Word |
| --- | --- |
| Backend | Python 3.14, FastAPI, SQLAlchemy, SQLite |
| Frontend QA | Nuxt 4.5.2, Vue 3.5.43 |
| Harness | Playwright 1.64.0, TypeScript 5.9.3 |
| Nền tảng | Windows, Chromium; cần ghi OS build/browser revision |
| Điều phối | workers=1; fullyParallel=false |
| Điều kiện ảnh | 1440×900; scale=1; light; en-US; UTC |
| Fixed date | 2026-10-12T12:00:00Z |
| Comparator | threshold=0.2; maxDiffPixelRatio=0.01; updateSnapshots=none |

Chưa có dependency harness local để xác minh lại phiên bản. Playwright upstream ở `mealie/tests/e2e` không phải bằng chứng đã có harness K10. Bổ sung Node/npm/pnpm, lockfile hash, browser revision và OS build vào manifest.

## 5. Thành viên và phân công

| Thành viên | MSSV | Phụ trách theo Word |
| --- | --- | --- |
| Nguyễn Đức Thành | 2312749 | Kiến trúc, ba data flow, test plan, báo cáo; review VR05–08 |
| K’ Nguyễn Quang Trường | 2312789 | Docker, frontend/backend, tài khoản QA, seed và onboarding |
| Nguyễn Viết Toàn | 2312777 | Playwright, auth, VR01–04, FLOW01–02, script và CI |
| Huỳnh Thành | 2312748 | VR05–08, FLOW03, baseline, diff, demo và kết quả |

Không tự gán tỷ lệ đóng góp hoặc ký duyệt thay thành viên.

## 6. Kiến trúc kiểm thử

```text
Source / dependency / cấu hình QA
                 |
     Backend Docker + SQLite <---- Frontend Nuxt
                 ^                         ^
                 |                         |
        API context: seed/FLOW    Playwright: AUTH01
                 |                         |
        Tạo dữ liệu tạm            storage state
                 |                         |
        UI đọc đối tượng       Route / nội dung / ổn định UI
                 |                         |
        Cleanup ID/slug          So ảnh baseline
                 |                         |
                 +----- JSON / HTML / ảnh / diff
```

Theo Word, API context đăng nhập riêng; page dùng state từ setup. Oracle kiểm tra route/nội dung, chờ loading/transition/font/ảnh, kiểm tra lỗi API/JS rồi so ảnh; sau capture kiểm tra lại route/lỗi. Chưa đọc lại helper thực thi của harness.

## 7. Cấu trúc repository

Local hiện có source upstream `mealie/`. Theo Word, repository QA là `https://github.com/2312777-rgb/mealie-nhom-23`; các đường dẫn dưới đây được ghi nhận theo tài liệu, chưa kiểm tra file gốc:

```text
mealie-nhom-23/
  README.md
  docker-compose.visual.yml
  playwright.config.ts
  project-lock.json
  package-lock.json
  scripts/verify-seed.mjs
  tests/auth.setup.ts
  tests/visual.spec.ts
  tests/business.spec.ts
  tests/__screenshots__/visual.spec.ts/   # 8 PNG theo Word
  docs/architecture.md
  docs/onboarding.md
  docs/baseline-review.md
  docs/defects.csv
  evidence/run-2026-10-16/results.json
  evidence/run-2026-10-16/report/index.html
  evidence/seed-idempotence.json
  evidence/demo-regression/
  evidence/demo-intentional/
  .github/workflows/visual-regression.yml
```

`.auth/user.json` là phiên đăng nhập sinh khi chạy, không commit hoặc chia sẻ công khai.

## 8. Trạng thái dự án

| Hạng mục | Theo Word ngày 16/10 | Kiểm chứng ở workspace hiện tại |
| --- | --- | --- |
| Source SUT | Đã pin | Đã xác nhận commit trùng |
| Harness | Đã triển khai | Chưa có checkout local |
| Bộ chính | 12 pass; 0 fail/skip/flaky | Chưa có JSON/HTML để đối chiếu |
| Visual / smoke / setup | 8 / 3 / 1, đều đạt | Chưa chạy lại |
| Typecheck/discovery | Đạt; 12 bài trong 3 file | Chưa chạy lại |
| Seed | unchanged=true; 10/3/10/2/7 | Chưa có artifact |
| Hai demo | Mismatch dự kiến; baselineChanged=false | Chưa có classification JSON |
| Duyệt baseline | Chờ nhóm xác nhận | Chưa có quyết định mới |
| CI | Có cấu hình; chưa URL run thành công | Chưa xác nhận |
| Máy thứ hai | Chưa chạy | Chưa xác nhận |

Word ghi suite bắt đầu `2026-10-16T06:37:23Z`, tổng thời gian 51,093 giây. Đây là số liệu lịch sử từ báo cáo, không phải lượt chạy mới. Không cộng hai demo vào 12 bài chính.

## 9. Phạm vi kiểm thử

| Mã | Màn hình / hoạt động | Nội dung đại diện theo Word |
| --- | --- | --- |
| AUTH01 | Login và storage state | Rời /login; main hiển thị |
| VR01 | /g/{groupSlug}/recipes/finder | No recipes found |
| VR02 | /g/{groupSlug}/recipes/timeline | QA Recipe 01 |
| VR03 | /g/{groupSlug}/recipes/categories | QA Breakfast |
| VR04 | /household/mealplan/planner/view | QA Recipe; đúng tuần |
| VR05 | /shopping-lists | QA Weekly Shopping; hai list |
| VR06 | /user/profile | Welcome |
| VR07 | /group/data/foods | QA Food 01 |
| VR08 | /household/mealplan/settings | Meal Plan hoặc Recipe Rules |
| FLOW01–03 | API tạo dữ liệu → UI đọc | Recipe, meal entry, shopping list |

Các pathname được dẫn theo Word; cần đối chiếu helper khi có harness. Chưa kết luận bao phủ tạo mới hoàn toàn bằng form UI, mọi quyền, mobile, đa browser, tải, bảo mật, accessibility hoặc toàn bộ unit/integration upstream.

## 10. Chiến lược kiểm thử

Quy trình: yêu cầu → kế hoạch → thiết kế → chuẩn bị → triển khai/review → thực thi → triage → retest/regression → tổng kết.

Tiêu chí vào: đúng baseline/config; UI/API sẵn sàng; tài khoản đúng group/household; seed đúng; dependency/browser và ảnh chuẩn hiện diện. Nếu login/seed fail, ghi setup failure/blocked, không tính bài chưa chạy là pass.

Tiêu chí đóng kỹ thuật đề xuất: bộ chính chạy đủ và đạt, cleanup xác nhận, artifact mở được, mọi mismatch được phân loại. Nhóm phải duyệt ảnh và ghi quyết định đối với rủi ro còn mở trước kết luận nghiệm thu. Đây là thiết kế bổ sung, chưa phải quyết định chấp nhận.

| REQ | Ca | Oracle chính |
| --- | --- | --- |
| REQ01 | AUTH01 | URL/main/state |
| REQ02 | VR01–03 | Route/text/ảnh trạng thái công thức |
| REQ03 | VR04, FLOW02 | Tuần, meal đại diện và ảnh |
| REQ04 | VR05, FLOW03 | Index hai list; list tạm hiển thị |
| REQ05 | VR06–08 | Nội dung và ảnh profile/food/settings |
| REQ06 | FLOW01 | API 201; GET name; UI tên; cleanup |
| REQ07 | Hai DEMO | Mismatch đúng ca; baseline không đổi |
| REQ08 | verify-seed | Số lượng đúng 10/3/10/2/7 |

Thêm Run ID, actual, artifact, defect và reviewer vào mỗi dòng khi có dữ liệu gốc.

Thiết kế mở rộng chưa chạy: shopping 0/1/2 list; biên tuần 11/12/18/19 tháng 10; tên tiếng Việt/tên dài; phiên chưa login/hợp lệ/hết hạn; meal có title/recipeId. Pairwise chưa được sinh/chạy; statement/branch coverage chưa đo.

Đã đọc local `CreatePlanEntry.id_or_title`: thiếu recipeId và title rỗng thì raise; có ít nhất một giá trị truthy thì qua validator này. UUID không chứng minh recipe tồn tại hoặc đúng quyền. Chuỗi toàn khoảng trắng truthy là ca cần xác nhận yêu cầu, chưa kết luận bug.

## 11. Business Flows

| Mã | Luồng theo Word | Expected / cleanup |
| --- | --- | --- |
| FLOW01 | POST /api/recipes, name=QA Smoke Recipe → slug → GET → UI chi tiết | 201; name đúng; UI có tên; finally DELETE slug |
| FLOW02 | POST /api/households/mealplans, date=2026-10-12, entryType=lunch, title=QA Smoke Lunch → planner | 201; UI có meal; finally DELETE entry.id |
| FLOW03 | POST /api/households/shopping/lists, name=QA Smoke Shopping → shopping index | 201; UI có list; finally DELETE list.id |

Đây là API ghi → UI đọc, chưa phải CRUD đầy đủ qua UI. Tên tạm cố định nên chạy tuần tự; sau interruption chỉ kiểm tra/dọn tài nguyên test quản lý. Không tự thêm Shopping Item thành một FLOW đã chạy.

## 12. Cài đặt và chạy Mealie

Word mô tả máy QA cũ ở `D:\mealie`, khác workspace hiện tại. Lệnh sau được chuẩn hóa cách xuống dòng từ Word, chưa thực thi trong lượt đối chiếu.

Terminal backend:

```powershell
Set-Location D:\mealie
docker compose -f mealie-nhom-23/docker-compose.visual.yml up -d --wait
```

Terminal frontend:

```powershell
Set-Location D:\mealie\frontend
$env:API_URL = 'http://localhost:19000'
pnpm.cmd run dev --no-fork
```

Terminal harness:

```powershell
Set-Location D:\mealie\mealie-nhom-23
npm.cmd run typecheck
npm.cmd run test:list
node scripts/verify-seed.mjs
npx.cmd playwright test
npm.cmd run test:demo
npm.cmd run test:visual:report
```

Máy mới cần checkout đúng source/harness, cài theo lockfile, cài Chromium, tạo `.env` từ mẫu, xác minh DB và bootstrap/seed đúng tài khoản. verify-seed có thể PUT lại dữ liệu chuẩn, chỉ dùng trên QA riêng. Lưu kết quả mới theo run riêng.

Theo Word, DB thực tế `/source/dev/data/mealie.db` khác mount `/app/data`. Trước recreate/chuyển DB cần sao lưu nhất quán và kiểm chứng phục hồi trên bản riêng. Chưa di chuyển hoặc kiểm chứng DB trong lượt này.

## 13. API Documentation

| Tài nguyên | URL dự kiến của môi trường QA, cần xác minh |
| --- | --- |
| UI | http://localhost:3000 |
| Swagger UI | http://localhost:19000/docs |
| ReDoc | http://localhost:19000/redoc |
| OpenAPI | http://localhost:19000/openapi.json |
| App information | http://localhost:19000/api/app/about |

Chưa xác nhận response hiện tại. Khi thu thập, ghi HTTP status, thời điểm và runtime baseline. Không dùng cổng 9091 upstream để mô tả lần QA cổng 19000.

## 14. OpenAPI Snapshot và các baseline

Ảnh giao diện là oracle chính của K10. OpenAPI snapshot chỉ là tài liệu tùy chọn hỗ trợ seed/FLOW; không bắt buộc bổ sung schema fuzzing để đủ khung 20 mục.

Tách bốn baseline: source, harness/dependency, môi trường/dữ liệu và ảnh được chấp nhận. Theo Word, tám PNG có tại `tests/__screenshots__/visual.spec.ts`, nhưng nhóm chưa xác nhận chính thức.

Nếu lưu OpenAPI, kèm nội dung, hash, thời điểm và commit runtime. Không dùng số paths/operations của repo K05 cho K10 khi chưa đo.

## 15. Playwright và quản lý ảnh

`threshold=0.2` điều chỉnh chênh màu để pixel bị coi là khác; `maxDiffPixelRatio=0.01` giới hạn tỷ lệ pixel khác. Hai số không biểu thị code coverage.

Quy trình duyệt: đúng dữ liệu/route/môi trường → đọc expected/actual/diff → xác định ý định → ghi quyết định từng ảnh → chỉ cập nhật khi được chấp nhận → chạy so sánh lại. Không dùng `test:visual:update` trong bước so sánh thường.

Theo Word, DEMO-R01 dịch main 48px, DEMO-I01 đổi nền thành `#1e40af`; mismatch đúng dự kiến, baselineChanged=false. Đây là sai lệch tổng hợp được chèn kiểm soát, không phải hai bug tự nhiên upstream.

Đề xuất chưa triển khai: so hash baseline trước/sau demo; đọc JSON theo đúng scenario thay vì tìm chuỗi toàn report; kiểm tra naturalWidth khi ảnh là nội dung bắt buộc.

## 16. Git Workflow và CI

Đề xuất nhánh `docs/*`, `test/*`, `fix/*`, `baseline/*` → PR có lý do/validation → review → merge. PR ảnh phải kèm diff và quyết định chấp nhận. Không đổi expected/ngưỡng chỉ để làm PASS.

Theo Word, validate trên Ubuntu chạy npm ci/typecheck/test discovery. Job visual chạy thủ công trên self-hosted Windows nhãn mealie-qa, gồm auth và tám visual; chưa chạy ba smoke. Chưa có URL CI run thành công.

Cổng chất lượng đề xuất: harness hợp lệ; preflight/seed đạt; visual không mismatch ngoài ý muốn; ba FLOW và cleanup đạt; có artifact và baseline được chấp nhận. Chỉ công bố CI bảo vệ đủ phạm vi sau khi workflow chạy đủ cổng và có run URL.

## 17. Security & Safety

- Không commit `.env`, token, mật khẩu hoặc `.auth/user.json`.
- Rà soát trace, network log, screenshot và HTML report trước chia sẻ.
- Dùng dữ liệu/tài khoản QA riêng; cleanup đúng ID/slug, không purge group.
- Theo dõi tài nguyên tạm khi interruption; kiểm tra cleanup trước chạy lại.
- Backup đúng DB trước thay lưu trữ; xác minh restore trên môi trường riêng.
- Nhóm quy định nơi lưu, người quản lý và thời hạn giữ evidence; chưa tự gán thời hạn.

## 18. Tài liệu dự án và biểu mẫu bổ sung

Word đã có quy trình, REQ/TC, tám visual, ba FLOW, thiết kế mở rộng, triage, baseline và CI. Các khoảng trống chủ yếu là biểu mẫu đóng hồ sơ và bằng chứng thực tế:

| Nội dung bổ sung | Bằng chứng vẫn cần |
| --- | --- |
| Manifest lần chạy | Metadata runtime và hash thực tế |
| Phiếu duyệt tám baseline | Người quyết định, ngày, lý do từng ảnh |
| Lệnh vận hành xuống dòng | Log chạy lại trên đúng máy |
| Danh mục artifact | JSON/HTML/PNG/classification gốc |
| Quy trình DB backup/restore/retest | Log kiểm chứng và trạng thái xử lý |
| Tái lập và CI | Kết quả máy thứ hai, URL CI |
| Truy vết REQ → TC → Run → artifact | Actual/evidence từng ca |

### Mẫu manifest — chưa có lần chạy mới

| Trường | Dữ liệu cần điền |
| --- | --- |
| Run ID / thời gian | RUN-YYYYMMDD-NN; ISO 8601 có timezone |
| Source / harness | Commit đầy đủ; working tree sạch hay có thay đổi |
| Runtime | Image ID/digest; DB_URL che secrets; UI/API URL |
| Dependency | Node/npm/pnpm; lockfile hash; Playwright/browser revision |
| Ảnh | OS/font/viewport/locale/timezone/fixed date |
| Dữ liệu | Tài khoản/group/household QA; kiểm tra 10/3/10/2/7 |
| Oracle | Commit/hash tám PNG; quyết định review |
| Kết quả | Pass/fail/skip/blocked/flaky; cleanup; demo ghi riêng |
| Artifact / reviewer | JSON/HTML/diff/log và người kiểm tra |

### Mẫu phiếu duyệt baseline

Mỗi VR01–VR08 ghi: file PNG thực; commit/hash; trạng thái dữ liệu; reviewer; ngày; chấp nhận/từ chối/chờ; lý do; yêu cầu hoặc PR liên quan. Hiện vẫn chờ quyết định, không ký duyệt thay nhóm.

Bản Word bổ sung giữ nội dung gốc, thêm phụ lục biểu mẫu/checklist. Khi nộp chính thức, nhóm cần tích hợp vào chương tương ứng và cập nhật mục lục/số trang trong Word.

## 19. Tiến độ tiếp theo

Chủ trì đề xuất dựa trên phân công Word; cần nhóm xác nhận:

| Ưu tiên | Công việc | Chủ trì đề xuất | Điều kiện đóng |
| --- | --- | --- | --- |
| Cao | Thu gom harness/artifact | Nguyễn Viết Toàn, Huỳnh Thành | Đúng commit; report/ảnh mở được |
| Cao | Kiểm chứng DB/lưu bền | K’ Nguyễn Quang Trường | Backup/restore và recreate có kiểm soát đạt |
| Cao | Duyệt tám baseline | Huỳnh Thành, Nguyễn Đức Thành | Quyết định từng ảnh và lý do |
| Trung bình | Khóa dependency/manifest | Nguyễn Viết Toàn, K’ Nguyễn Quang Trường | Metadata/hash thực tế |
| Trung bình | Chạy máy thứ hai | K’ Nguyễn Quang Trường cùng nhóm | Kết quả mới, giải thích sai khác |
| Trung bình | Mở rộng ca rủi ro | Chủ scenario | Test/expected được review; kết quả riêng |
| Khi runner sẵn sàng | CI và smoke nếu chọn gate đầy đủ | Nguyễn Viết Toàn | Run URL/artifact; phạm vi từng job rõ |
| Trước nộp | Đồng bộ báo cáo | Nguyễn Đức Thành cùng nhóm | Evidence truy vết; baseline/rủi ro được chốt |

Word ghi mốc giữa kỳ 20/10/2026; cuối kỳ trước bảo vệ một tuần, chưa xác nhận ngày thi. Đây là lịch theo tài liệu, chưa xác minh lịch mới với giảng viên.

## 20. Ghi chú và giới hạn

- Đã đọc Word đúng và xác nhận commit SUT local trùng báo cáo K10.
- Không dùng file Mặc Studio gửi nhầm hoặc số liệu/phân công nhóm K05.
- Chưa chạy Docker, Playwright, seed, demo hay CI; không có kết quả mới.
- 12/12 là pass rate của bộ đã chọn theo Word, không phải code coverage.
- Baseline chưa duyệt, DB/mount, dependency drift và máy thứ hai vẫn còn mở.
- Bổ sung tài liệu không đồng nghĩa triển khai script, sửa DB hoặc duyệt ảnh.
- Nguồn: Word do người dùng cung cấp và source local tại commit đã nêu. Đường dẫn repo harness/evidence được dẫn theo Word, chưa truy cập lại trong lượt này.
