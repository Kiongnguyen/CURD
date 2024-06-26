# CURD

## Note fullstask - CRUD -be vs fe

>mySQL <---> Express <---> React js

### 1.mySQL: 
Database SQL được sử dụng để tạo bảng lưu trữ dữ liệu cho ứng dụng web.

- Ứng dụng chạy trên Docker, sử dụng Docker Compose để khởi chạy:
```git 
docker compose -f mysql.yml -p nodejs-sql up -d 
```
- Files MySQL:	
```yaml
    version: '3.8'
	services:
		db-mysql:
		image: mysql:5.7.40
		restart: always
		environment:
		- MYSQL_DATABASE=hoidanit
		- MYSQL_ROOT_PASSWORD=123456
		ports:
		- "3307:3306"
```
		
### 2.Express: 
Chạy trên nền node_js:

* trên BE vẫn hiển thị render ra trên localhots:8080 ===> có thể nói đây là SSR
* trong ứng dụng này cần chú ý vào ==> app :chứa thư mục chính còn lại là các thư mục bổ trợ không tự tìm hiểu, xem qua các files apps:
	
		1/sever.js: 
		
			*/ import thư viện: 
				*/dotenv ==> tạo môi trường cho env
				*/express ==> thư viện tạo BE
				*/cors ==> thự viện dùng để làm việc với database
				*/path ==> tạo đường truy cập 
				*/app ==> tạo sever
				
			*/ add thư viện vào sever: 
				*/cors 
				*/express.json ==> trả về dữ liệu dạng json
				*/ejs ==> 1 dạng view engine 
				*/extended được đặt thành true, nó cho phép các đối tượng lồng nhau trong dữ liệu được mã hóa theo URL.
				
			*/db.sequelize. ==> Dòng này đồng bộ hóa các mô hình Sequelize với cơ sở dữ liệu. Nó đảm bảo rằng cấu trúc cơ sở dữ liệu phù hợp với cấu trúc được định nghĩa trong các mô hình Sequelize của bạn.
							==> Phương thức sync() tự động tạo bảng cơ sở dữ liệu dựa trên các định nghĩa mô hình của bạn nếu chúng không tồn tại. Nếu các bảng đã tồn tại, nó có thể sửa đổi các bảng hiện có để phản ánh bất kỳ thay đổi nào trong các định nghĩa mô hình.
					
			*/require("./routes/tutorial.routes")(app);:
	
				*/Đây là cách gọi một module từ một tệp JavaScript khác trong Node.js.
				*/Module được gọi là "./routes/tutorial.routes". Điều này ngụ ý rằng có một thư mục có tên routes trong thư mục hiện tại, và trong đó có một tệp có tên tutorial.routes.js.
				*/Khi module được gọi, nó được truyền một tham số, trong trường hợp này là app, đại diện cho đối tượng ứng dụng Express.
				*/Điều này cho phép module "tutorial.routes" truy cập và sử dụng app để định nghĩa các tuyến đường cho ứng dụng.
				
			*/require("./routes/tutorial.api")(app);
				
				*/Module được gọi là "./routes/tutorial.api", ngụ ý rằng có một tệp có tên tutorial.api.js trong thư mục routes.
				*/Cũng giống như trên, module này cũng được truyền app như một tham số, cho phép nó cấu hình các API cho ứng dụng Express.
		
		2/routes:
			
			*/ tutorial.routes ==> hiển thị trực tiếp trên sever
			*/ tutorial.api ==> xuất api cho frontend
			
		3/views: 
			hiển thị giao diện 
			 
		4/config: db.config --> tạo liên kết với dữ liệu database vì vậy cái này có thể thay đổi tùy ý 
		
		5/controllers: 
		
			*/tutorial.controllers ==> các đường routes liên kết trức tiếp với các thao tác trực tiếp 
			
			*/api.controlles ==> liên hệ trực tiếp với tutorial.api khi bên FE lấy theo đường routes ấy sẽ thu được mảng JSON
			
		6/models: dùng để tạo liên kết 
		
		

### 3.React js: 
Dùng cho frontend  chạy theo CSR với ý nghĩa ơ clien sẽ thao tác in hết dữ liệu ra sử lí thao tác render từ API, vì vậy cần chú ý thao tác call API

	*/các file public, assets, git, node_modules tự tìm hiểu 
	*/Phân tích files quan trọng:
	
		1/.evn:  
			VITE_BACKEND_URL=http://localhost:8080/api/tutorials ==> liên kết với BE
			VITE_APP_PORT=3000 ==> PORT của FE
			
		2/index ==> render dữ liệu ra browes 
		
		3/main.js ==> liên kết với dom html
		
		4/App.scss ==> file css của app
		
		5/axios.js :
		
			Đoạn mã trên sử dụng thư viện Axios để tạo một instance (đối tượng) của Axios để thực hiện các yêu cầu HTTP. Hãy phân tích từng phần của mã:

			1. `import axios from 'axios';`:
			- Dòng này import Axios từ thư viện 'axios' để sử dụng trong mã.

			1. `const instance = axios.create({ baseURL: import.meta.env.VITE_BACKEND_URL });`:
			   - Đoạn này tạo một instance của Axios với cài đặt cụ thể.
			   - `axios.create()` tạo một instance mới của Axios với các tùy chọn được cung cấp.
			   - `baseURL` được thiết lập để là giá trị của biến môi trường `VITE_BACKEND_URL`. Biến môi trường này thường được sử dụng để xác định URL của backend server.

			2. `instance.interceptors.request.use(function (config) { ... });`:
			   - Interceptor này được sử dụng để thực hiện các thao tác trước khi yêu cầu được gửi đi.
			   - Trong hàm callback, bạn có thể thực hiện bất kỳ thay đổi nào bạn muốn trên cấu hình của yêu cầu (ví dụ: thêm các headers, xử lý dữ liệu trước khi gửi yêu cầu, v.v.).

			3. `instance.interceptors.response.use(function (response) { ... });`:
			   - Interceptor này được sử dụng để xử lý các phản hồi từ server trước khi chúng được trả về cho caller.
			   - Trong hàm callback, bạn có thể thực hiện các thao tác trên dữ liệu phản hồi (ví dụ: xử lý dữ liệu phản hồi, kiểm tra mã trạng thái của phản hồi, v.v.).

			4. `export default instance;`:
			   - Dòng này xuất instance của Axios đã được tạo để có thể được sử dụng trong các module khác của ứng dụng.

			Tóm lại, đoạn mã này thiết lập một instance của Axios với các interceptors để xử lý yêu cầu và phản hồi, sau đó xuất nó để sử dụng trong ứng dụng của bạn.
			
			
		6/ModalUpsert.jsx
		
			Đoạn mã React của bạn là một component `ModalUpsert` được thiết kế để tạo và cập nhật thông tin bài hướng dẫn. Dưới đây là mô tả chi tiết về chức năng của nó:

				1. **Props Nhận Được**: Component nhận các props như `show`, `setShowModal`, `action`, `dataModal`, và `getListTutorials`.

				2. **State**: Component sử dụng hook `useState` để lưu trữ trạng thái local cho đối tượng bài hướng dẫn, bao gồm các thuộc tính như `id`, `title`, `description`, và `published`.

				3. **Effect**: Có một hook effect được sử dụng để theo dõi sự thay đổi của `dataModal`. Nếu có dữ liệu trong `dataModal`, nó sẽ cập nhật trạng thái local (`tutorial`).

				4. **Xử Lý Thay Đổi Trường Nhập**: Hàm `handleInputChange` được sử dụng để cập nhật trạng thái khi có sự thay đổi trong các trường nhập liệu.

				5. **Xử Lý Gửi Form**: Hàm `handleSubmit` được gọi khi người dùng nhấn vào nút "Lưu Thay Đổi". Nếu hành động là 'C' (Tạo mới), nó thực hiện một yêu cầu POST, ngược lại nếu hành động khác, nó thực hiện yêu cầu PUT. Sau khi yêu cầu được thực hiện, nó xóa form, làm mới danh sách các bài hướng dẫn, và ẩn modal.

				6. **Cấu Trúc Modal**: Component render một modal Bootstrap. Tùy thuộc vào hành động, nó hiển thị một tiêu đề khác nhau ("Tạo mới người dùng" hoặc "Cập nhật người dùng"). Bên trong phần thân của modal, có các trường nhập liệu cho tiêu đề, mô tả và một dropdown select cho trạng thái đã xuất bản.

				7. **Nút Đóng**: Có một nút đóng ở phần chân của modal, khi nhấn vào nó sẽ ẩn modal.

				8. **Nút Lưu Thay Đổi**: Nút này kích hoạt hàm `handleSubmit` khi được nhấn.

				Tổng quan, component này cung cấp một modal để tạo và cập nhật thông tin bài hướng dẫn, cho phép người dùng nhập tiêu đề, mô tả, và trạng thái xuất bản, với chức năng xử lý gửi form và đóng modal.
		
		5/App.jsx ==> trang render chính của reacts
		
			Đoạn mã JavaScript bạn đã cung cấp đang sử dụng React để tạo ra một ứng dụng quản lý danh sách hướng dẫn. Dưới đây là mô tả chi tiết về chức năng của mã:

			5. **Import các module và component cần thiết**:
			   - `axios`: Được sử dụng để giao tiếp với API.
			   - `useEffect`, `useState` từ `react`: Sử dụng để quản lý side effects và trạng thái trong React.
			   - `App.scss`: File CSS cho giao diện ứng dụng.
			   - `ModalUpsert`: Component ModalUpsert được import từ file `ModalUpsert.js`.

			6. **Component `App`**:
			   - Sử dụng các hook `useState` để quản lý trạng thái của danh sách hướng dẫn, hiển thị modal, hành động hiện tại và dữ liệu modal.
			   - Sử dụng hook `useEffect` để lấy danh sách hướng dẫn khi component được mount lần đầu.
			   - `getListTutorials`: Hàm lấy danh sách hướng dẫn từ API bằng cách sử dụng Axios và cập nhật trạng thái của danh sách.
			   - `handleDelete`: Hàm xử lý xóa một hướng dẫn dựa trên id, với xác nhận từ người dùng.
			   
			7. **Render**:
			   - Hiển thị nút "Add new" để mở modal thêm mới hướng dẫn.
			   - Hiển thị danh sách hướng dẫn trong một bảng.
			   - Mỗi dòng trong bảng bao gồm thông tin của một hướng dẫn cùng với nút "Edit" để chỉnh sửa và nút "Delete" để xóa.
			   
			8. **ModalUpsert Component**: Được render để tạo hoặc chỉnh sửa hướng dẫn.

			Trong tổng thể, đoạn mã này xây dựng một ứng dụng React đơn giản để quản lý danh sách hướng dẫn, cho phép thêm, sửa, xóa các mục trong danh sách thông qua giao diện người dùng.
				
