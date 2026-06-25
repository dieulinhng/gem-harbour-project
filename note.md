- [x] Trong thực tế khi mình A/B testing, thì sau **bao lâu** từ khi nhận kết quả A/B test thì mình sẽ kết luận là version mới tốt hơn (hoặc kém hơn) so với version cũ? Nếu tốt hơn thì có launch production luôn không?
=> nên check
- [x] Công ty đã có alert model nào cho việc drop RR chưa?

ƯU TIÊN HỎI NGAY VÀ LUÔN
- [x] Công ty có dùng COUNT(user_pseudo_id) để tính tổng số lượng user cài đặt trong ngày không?
=> count distinct
- [x] Công ty có tham khảo benchmark trung bình trong ngành game (của từng thể loại) để xác định xem game của mình có đang tốt không? Hay chỉ dựa vào số liệu lịch sử của chính công ty + kỳ vọng đối với tựa game đó để xem là RR có đang thực-sự-tốt?
=> gắn sự giảm với business xem cái sự giảm này có THỰC SỰ LÀ VẤN ĐỀ, nên xem cả các chỉ số khác như CPI, ROAS, LTV => nó có thực sự xấu không?
=> TÌM RA NGUYÊN NHÂN GIẢM => suggest?
- [x] Trong thực tế thì công ty có nhiều thay đổi ngay trong Soft Launch không? 
- [ ] Các anh chị đã từng gặp tình huống này chưa, nếu rồi thì anh chị xử lý thế nào?

(product) Kiểm tra cohort behavior của user trước và sau thay đổi => so sánh 2 nhóm đó xem có sự khác biệt gì không

UA -> pre-install: không ảnh hưởng bởi product (version, ab) => nếu chất lượng user pre-install không tốt thì do thằng UA (CPM, CPI, v.v)
nên từ funnel users

campaign mới đang chạy trên các country nào? => 
có phải tất cả country đều giảm RR không?


user trước tác động/sau tác động => break theo country (country nào thay đổi, country nào không) => break theo network, campaign
+ nếu country + network cùng giảm thì nó sẽ là vấn đề ở product