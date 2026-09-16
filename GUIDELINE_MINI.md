# Mini Guideline -- Nhóm

**Nhóm:** \_\_\_\_\_\_\_\_ \| **Người gán:** \_\_\_\_\_\_\_\_ \|
**Ngày:** \_\_\_\_\_\_\_\_

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần
> bạn dừng lại hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc

-   Sử dụng 17 keypoint COCO, đúng tên và thứ tự theo file `.SVG` chung.
-   Mỗi người trong ảnh phải có đủ 17 keypoint.
-   Trái/phải tính theo cơ thể người, không theo hướng trong ảnh.
-   Khớp bị che nhưng còn trong khung ảnh: `v = 1`, vẫn đặt điểm ở vị
    trí ước lượng.
-   Khớp nằm ngoài mép ảnh: `v = 0`, không đặt điểm.
-   Không sử dụng `Hidden (h)` vì thuộc tính này không được lưu vào
    file.

------------------------------------------------------------------------

## 2. Luật của nhóm

  -----------------------------------------------------------------------
  Tình huống              Luật nhóm bạn chọn      Vì sao
  ----------------------- ----------------------- -----------------------
  **Hông của người mặc    Đặt keypoint hông theo  Keypoint cần phản ánh
  quần áo dài**           vị trí ước lượng của    vị trí giải phẫu, không
                          khớp hông thực tế, dựa  phải vị trí bề mặt quần
                          trên cấu trúc cơ thể và áo.
                          vị trí hai chân. Nếu bị 
                          quần áo che nhưng vẫn   
                          nằm trong ảnh thì đặt   
                          `v = 1`.                

  **Tai bị tóc hoặc mũ    Nếu vẫn ước lượng được  Đảm bảo đủ 17 keypoint
  bảo hiểm che một phần** vị trí tai, đặt         và thống nhất cách xử
                          keypoint tại vị trí tai lý các điểm bị che.
                          thực tế và gán `v = 1`. 
                          Nếu tai bị che hoàn     
                          toàn nhưng vẫn xác định 
                          được vị trí tương đối,  
                          tiếp tục đặt điểm ước   
                          lượng.                  

  **Người bị cắt ở mép    Các keypoint nằm ngoài  Phân biệt rõ khớp bị
  ảnh, chỉ thấy từ hông   khung ảnh gán `v = 0`   che và khớp nằm ngoài
  trở lên**               và không đặt điểm. Các  ảnh.
                          keypoint còn trong ảnh  
                          vẫn được gán bình       
                          thường.                 

  **Cổ tay nằm sau tay    Nếu cổ tay bị vật thể   Tránh bỏ sót keypoint
  lái hoặc sau thân       che nhưng vẫn nằm trong khi người hoặc vật thể
  mình**                  khung ảnh, ước lượng vị che khuất một phần cơ
                          trí khớp và gán         thể.
                          `v = 1`. Nếu không thể  
                          xác định chính xác,     
                          đánh dấu để thảo luận.  

  **Hai người chồng lên   Gán keypoint riêng cho  Tránh gán nhầm keypoint
  nhau**                  từng người. Với các     của người này sang
                          khớp bị che, ước lượng  người khác.
                          vị trí dựa trên tư thế, 
                          các khớp liên quan và   
                          cấu trúc cơ thể.        

  **Người nhỏ đến mức nào Vẫn gán nhãn nếu có thể Đảm bảo dữ liệu có tính
  thì không gán nữa**     xác định đó là người và nhất quán, tránh gán
                          ước lượng được vị trí   nhãn tùy tiện cho các
                          các khớp. Chỉ xem xét   đối tượng không thể
                          không gán khi người quá nhận diện.
                          nhỏ hoặc quá mờ đến mức 
                          không thể xác định tư   
                          thế và vị trí khớp.     
  -----------------------------------------------------------------------

> **Ảnh mẫu cần bổ sung:** Chụp màn hình CVAT tương ứng với từng tình
> huống khi gặp trong quá trình gán nhãn.

------------------------------------------------------------------------

## 3. Ba ca mơ hồ đã gặp

### Ca 1 -- Ảnh `train_03.jpg`, người thứ 1, khớp hông

-   **Mơ hồ ở chỗ nào:** Người trong ảnh mặc áo khoác dài. Vị trí hông
    không thể quan sát rõ do bị quần áo và các vật thể xung quanh che
    khuất.
-   **Bạn quyết thế nào:** Đặt keypoint hông tại vị trí ước lượng dựa
    trên vị trí thân người, hướng của hai chân và cấu trúc cơ thể. Gán
    `v = 1`.
-   **Vì sao:** Khớp bị che nhưng còn trong ảnh vẫn phải được gán nhãn
    và đánh dấu `v = 1`.
-   **Nếu người khác quyết ngược lại thì model học sai cái gì:** Mô hình
    có thể học sai vị trí giải phẫu của hông nếu đặt điểm theo mép áo
    hoặc bỏ qua keypoint.

### Ca 2 -- Ảnh `train_04.jpg`, người thứ 1, khớp tai

-   **Mơ hồ ở chỗ nào:** Người đi xe máy đội mũ bảo hiểm. Vị trí tai bị
    che hoàn toàn hoặc không thể quan sát rõ.
-   **Bạn quyết thế nào:** Ước lượng vị trí tai dựa trên vị trí đầu,
    hướng mặt và cấu trúc mũ bảo hiểm. Đặt keypoint và gán `v = 1`.
-   **Vì sao:** Tai vẫn nằm trong khung ảnh nhưng bị che khuất nên không
    được bỏ điểm.
-   **Nếu người khác quyết ngược lại thì model học sai cái gì:** Mô hình
    có thể học sai mối quan hệ giữa tai, mắt và mũi nếu bỏ keypoint hoặc
    đặt tùy tiện.

### Ca 3 -- Ảnh `train_11.jpg`, người thứ 1, khớp cổ tay

-   **Mơ hồ ở chỗ nào:** Cổ tay của người đi xe máy bị tay lái và các bộ
    phận xe che khuất.
-   **Bạn quyết thế nào:** Ước lượng vị trí cổ tay dựa trên hướng cẳng
    tay, vị trí khuỷu tay và tay lái. Đặt keypoint và gán `v = 1`.
-   **Vì sao:** Cổ tay vẫn nằm trong khung ảnh nhưng bị vật thể che
    khuất.
-   **Nếu người khác quyết ngược lại thì model học sai cái gì:** Mô hình
    có thể học sai cấu trúc cánh tay nếu đặt cổ tay theo vị trí tay lái
    thay vì vị trí khớp thực tế.

------------------------------------------------------------------------

## 4. Sau khi so visibility report với bạn cùng nhóm

-   **Khớp lệch `%v = 1` nhiều nhất:**
    \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
-   **Bạn:** \_\_\_\_\_\_\_\_% \| **Bạn cùng nhóm:** \_\_\_\_\_\_\_\_%
-   **Nguyên nhân:** ☐ Guideline chưa rõ ☐ Một trong hai bên gán sai

### Tự diễn giải 3 ca mơ hồ

1.  **Ca hông bị che bởi quần áo:** Sự khác biệt có thể đến từ việc một
    người đặt điểm theo vị trí giải phẫu, trong khi người còn lại đặt
    theo bề mặt quần áo.
2.  **Ca tai bị che bởi mũ bảo hiểm:** Cần thống nhất cách ước lượng vị
    trí tai khi không nhìn thấy rõ và không được tự ý bỏ điểm.
3.  **Ca cổ tay bị che bởi tay lái:** Cần thống nhất cách suy luận vị
    trí cổ tay dựa trên hướng cẳng tay và các khớp liên quan.

------------------------------------------------------------------------

## 5. Luật mới bổ sung vào mục 2 sau khi thống nhất

  -----------------------------------------------------------------------
  Luật mới                            Nội dung
  ----------------------------------- -----------------------------------
  **Ước lượng khớp bị che**           Khi khớp bị che nhưng còn trong
                                      ảnh, luôn đặt điểm dựa trên cấu
                                      trúc cơ thể và các khớp liên quan,
                                      đồng thời gán `v = 1`.

  **Phân biệt khớp bị che và khớp     Khớp bị che vẫn đặt điểm. Khớp nằm
  ngoài ảnh**                         ngoài mép ảnh thì gán `v = 0` và
                                      không đặt điểm.

  **Xử lý các khớp khó xác định**     Nếu không thống nhất được vị trí
                                      khớp sau 10 giây, đánh dấu tình
                                      huống để cả nhóm thảo luận và bổ
                                      sung ảnh mẫu vào guideline.
  -----------------------------------------------------------------------

> **Lưu ý:** Trước khi nộp, cần đối chiếu lại từng ảnh với lịch sử gán
> nhãn thực tế trên CVAT để đảm bảo các ca mơ hồ đúng là những tình
> huống đã gặp và thực sự mất hơn 10 giây để phân vân.
