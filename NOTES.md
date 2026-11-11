# Ghi chú phân tích · Day 04

Họ và tên: Nguyễn Cao Minh  
MSSV: 2A202604099

## 1. Case được chọn

- ID case: **H1**
- Câu trả lời của agent: 
```
### Incident GitHub: h02n9572mx9y\n- **Tên**: Incident with Actions\n- **Trạng thái**: Resolved\n- **Thời gian bắt đầu**: 2026-11-04 17:40 UTC\n- **Cập nhật gần nhất**: 2026-11-07 01:10 UTC\n- **Component liên quan**: Actions\n- **Mô tả**:\n  - Bắt đầu điều tra về sự giảm khả dụng cho Actions vào 17:40 UTC.\n  - Tình trạng \"Monitoring\" được xác nhận vào 18:07 UTC, khi sự cố đã giảm nhẹ.\n  - Vấn đề được giải quyết vào 18:16 UTC với thông báo rằng có một bản tóm tắt liên quan đến sự cố khác ([link](https://www.githubstatus.com/incidents/xr3myzj7ms51)).\n  \n### Incident Cloudflare: yaa0witbl2me\n- **Tên**: Workers Builds Issues\n- **Trạng thái**: Resolved\n- **Thời gian bắt đầu**: 2026-11-05 19:51 UTC\n- **Cập nhật gần nhất**: 2026-11-05 20:20 UTC\n- **Component liên quan**: Workers Builds\n- **Mô tả**:\n  - Sự cố được xác định vào 19:51 UTC, ảnh hưởng đến một số khách hàng.\n  - Tình trạng \"Monitoring\" được báo cáo vào 19:55 UTC khi các bản build đang được xử lý.\n  - Vấn đề được giải quyết vào 20:20 UTC với thông báo rằng thời gian xếp hàng đã trở lại bình thường ([link](https://www.cloudflarestatus.com/incidents/yaa0witbl2me)).\n\n### So sánh\n- **Thời gian**: \n  - GitHub: 2026-11-04 (17:40 UTC)\n  - Cloudflare: 2026-11-05 (19:51 UTC)\n- **Trạng thái**: Cả hai đều đã được giải quyết.\n- **Component**: \n  - GitHub liên quan đến \"Actions\".\n  - Cloudflare liên quan đến \"Workers Builds\".\n\n### Kết luận\nDựa trên dữ liệu từ hai incident, không có bằng chứng rõ ràng nào cho thấy sự cố của GitHub có liên quan đến sự cố của Cloudflare. Các thời gian của sự cố là khác nhau và phạm vi ảnh hưởng cũng không giống nhau. GitHub tập trung vào vấn đề với Actions, trong khi Cloudflare đang xử lý các vấn đề liên quan đến Workers Builds. \n\nNguồn dữ liệu: \n- [GitHub Incident](https://stspg.io/7pwv28hvkmz8)\n- [Cloudflare Incident](https://www.cloudflarestatus.com/incidents/yaa0witbl2me)
```
- Điểm ở lần chạy 1: **60/100**

## 2. Bằng chứng trong trace

Trace H1 trong `eval_before.json`:

```json
"trace": [
          {
            "event": "model",
            "step": 1,
            "finish": "tool_calls"
          },
          {
            "event": "tool",
            "step": 1,
            "name": "get_incident_details",
            "arguments": "{\"provider\": \"github\", \"incident_id\": \"h02n9572mx9y\"}",
            "result": {
              "ok": true,
              "provider": "github",
              "captured_at": "2026-11-09T03:50:47.065020+00:00",
              "id": "h02n9572mx9y",
              "name": "Incident with Actions",
              "status": "resolved",
              "created_at": "2026-11-04T17:40:33.675Z",
              "updated_at": "2026-11-07T01:10:18.506Z",
              "components": [
                "Actions"
              ],
              "updates": [
                {
                  "status": "resolved",
                  "created_at": "2026-11-04T18:16:08.361Z",
                  "body": "Please refer to the combined summary in this related incident: https://www.githubstatus.com/incidents/xr3myzj7ms51",
                  "truncated": false
                },
                {
                  "status": "monitoring",
                  "created_at": "2026-11-04T18:07:51.114Z",
                  "body": "The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.",
                  "truncated": false
                },
                {
                  "status": "investigating",
                  "created_at": "2026-11-04T17:40:33.887Z",
                  "body": "We are investigating reports of degraded availability for Actions",
                  "truncated": false
                }
              ],
              "updates_total": 4,
              "url": "https://stspg.io/7pwv28hvkmz8"
            }
          },
          {
            "event": "tool",
            "step": 1,
            "name": "get_incident_details",
            "arguments": "{\"provider\": \"cloudflare\", \"incident_id\": \"yaa0witbl2me\"}",
            "result": {
              "ok": true,
              "provider": "cloudflare",
              "captured_at": "2026-11-09T03:50:47.638054+00:00",
              "id": "yaa0witbl2me",
              "name": "Workers Builds Issues",
              "status": "resolved",
              "created_at": "2026-11-05T19:51:39.176Z",
              "updated_at": "2026-11-05T20:20:10.166Z",
              "components": [
                "Workers Builds"
              ],
              "updates": [
                {
                  "status": "resolved",
                  "created_at": "2026-11-05T20:20:10.166Z",
                  "body": "Queue times have returned to normal.",
                  "truncated": false
                },
                {
                  "status": "monitoring",
                  "created_at": "2026-11-05T19:55:51.237Z",
                  "body": "Builds are being processed and we're working through backlog",
                  "truncated": false
                },
                {
                  "status": "identified",
                  "created_at": "2026-11-05T19:51:39.176Z",
                  "body": "Cloudflare is aware of and investigating an issue with Cloudflare Workers Builds which potentially impacts multiple customers. Further detail will be provided as more information becomes available.",
                  "truncated": false
                }
              ],
              "updates_total": 3,
              "url": "https://www.cloudflarestatus.com/incidents/yaa0witbl2me"
            }
          },
          {
            "event": "model",
            "step": 2,
            "finish": "stop"
          }
        ],
```

Các step agent gọi tool:

```text
Step 1: get_incident_details(github, h02n9572mx9y)
        Update: Please refer to the combined summary in this related incident: https://www.githubstatus.com/incidents/xr3myzj7ms51
Step 2: get_incident_details(cloudflare, yaa0witbl2me)
Step 3: finish = stop → completed
```

**Nguyên nhân lỗi có thể xảy ra:** Model có thể đã coi thông tin từ hai incident ban đầu là đủ để trả lời, nên chỉ đưa liên kết xr3myzj7ms51 vào câu trả lời mà không gọi tool để tra cứu incident với id xr3myzj7ms51. Loop ban đầu chấp nhận finish == "stop" ngay, chưa kiểm tra còn incident liên quan chưa đọc, nên agent kết thúc khi chưa thu thập đủ bằng chứng về nguyên nhân và ảnh hưởng.

Trace xác nhận link liên kết đã được trả đầy đủ (`"body": "Please refer to the combined summary in this related incident: https://www.githubstatus.com/incidents/xr3myzj7ms51"`, `truncated: false`), nhưng không có tool call đọc xr3myzj7ms51. Vì vậy, lỗi chính là dừng điều tra quá sớm, không phải do thiếu liên kết hay do đọc Cloudflare trước incident liên quan.

## 3. Thay đổi đã thực hiện

- Bổ sung prompt yêu cầu đọc incident liên quan trước khi kết luận về nguyên nhân.
- Thêm `related_incidents()` lấy provider và incident ID từ URL trong updates. Chỉ nhận đường dẫn incident trên host GitHub/Cloudflare được hỗ trợ, không mở URL tùy ý.
- Dùng `linked_incidents` và `attempted_incidents` theo dõi liên kết chưa thử đọc. Khi model định kết thúc sớm, ghi `validation_unread_related_incident` và yêu cầu gọi tool tiếp. Đánh dấu cả lần tra cứu thất bại để tránh buộc retry lặp lại.
- Lưu URL incident đọc thành công trong `incident_urls`. Nếu câu trả lời thiếu nguồn, ghi `validation_missing_source` và yêu cầu sửa.

## 4. So sánh hai lần chạy


| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---:|---:|
| Điểm tổng | 80.68 | 83.41 |
| Điểm case đã chọn (H1) | 60 | 80 |
| Số model calls | 51 | 57 |
| Token của agent, toàn bộ case | 46,575 | 71,308 |
| Chi phí agent theo `usage.cost` | 0.01137045 | 0.01486935 |
| Model calls cho H1 | 2 | 5 |


- Lỗi đã giải quyết: Trong H1, agent đã theo liên kết từ tool call đầu tiên để gọi `get_incident_details()` đọc thêm incident `xr3myzj7ms51`, bổ sung nguyên nhân và ảnh hưởng của GitHub. Agent gọi tool ở 2 model steps khác nhau, tổng cộng 3 tool calls. Điểm H1 tăng 60 -> 80, điểm tổng tăng 80.68 -> 83.41, đổi lại số calls, token và chi phí tăng.
- Lỗi còn lại hoặc mới xuất hiện: H7 vẫn suy đoán Cloudflare hoạt động bình thường dù tool bị timeout; lỗi này có ở cả hai lần chạy, và lần 2 giảm từ 65 -> 45 điểm. Với case A7, agent khẳng định Spectrum “hiệu suất suy giảm tại Việt Nam”, trong khi tool chỉ cung cấp trạng thái component và incident tại Phoenix. Chưa có bằng chứng xác nhận phạm vi Việt Nam. Với case H6, agent chưa nêu rõ nguyên nhân của incident và nguồn gốc chính xác của thông tin đã sử dụng.

## 5. Giới hạn
- Mỗi phiên bản mới chạy eval một lần nên mức tăng điểm chưa đủ chứng minh cải tiến ổn định; câu trả lời của agent và điểm LLM judge có thể thay đổi giữa các lần chạy. Cần chạy lặp cả hai phiên bản với cùng model, judge, snapshot và max_steps, rồi so sánh điểm trung bình và độ dao động để có so sánh reliable hơn.
- Judge có thể chấm sai nên cần đối chiếu answer và trace.
- Agent có thể hết max_steps khi cần đọc thêm hoặc sửa câu trả lời. Có URL nguồn không bảo đảm mọi kết luận đều đúng.