# Service Boundary của nhóm

## 1. Thông tin nhóm

- **Tên nhóm**: 5A
- **Lớp**: CNTT-1709
- **Thành viên**: Nguyễn Trung Kiên, Nguyễn Văn Phú, Nguyễn Đức Mạnh
- **Service nhóm phụ trách**: **A5 - Analytics**
- **Sản phẩm tổng thể của lớp**: Smart Campus Ecosystem - Product A

## 2. Actor

- Ban quản lý campus (Quản trị viên)
- Giảng viên và Sinh viên
- Hệ thống Dashboard người dùng
- Các service nội bộ khác (IoT Ingestion, AI Vision, Access Gate)

## 3. System Boundary

**Nhóm em xây phần nào?**
- Xây dựng dịch vụ tổng hợp và phân tích dữ liệu (Analytics Service)

**Phần nhóm kiểm soát:**
- Analytics Service (xử lý, phân tích, báo cáo)
- Data Warehouse / Data Lake
- Dashboard & Visualization (tích hợp Grafana)

**Phần nhóm chỉ tích hợp:**
- Dữ liệu từ A1 (IoT Ingestion)
- Dữ liệu từ A2 (Access Gate)
- Dữ liệu từ A3 (AI Vision)
- Dữ liệu từ Core Business

## 4. Service Boundary

**Service của nhóm có trách nhiệm:**
- Thu thập và tổng hợp dữ liệu từ nhiều service
- Xử lý, làm sạch, transform dữ liệu
- Phân tích real-time và batch processing
- Tính toán metrics, KPIs, xu hướng
- Cung cấp insight, báo cáo và cảnh báo

**Service KHÔNG làm:**
- Không thu thập trực tiếp dữ liệu từ cảm biến/camera
- Không kiểm soát ra vào (Access Gate)
- Không xử lý luồng camera thời gian thực

## 5. Input / Output

### Input
- Event dữ liệu từ **A1 - IoT Ingestion**
- Dữ liệu nhận diện từ **A3 - AI Vision**
- Dữ liệu ra vào từ **A2 - Access Gate**
- Dữ liệu nghiệp vụ từ Core Business

### Output
- Dashboard metrics & KPIs
- Báo cáo phân tích (daily/weekly)
- Insight và cảnh báo thông minh
- Dữ liệu tổng hợp cho các service khác

## 6. API dự kiến

| Method | Endpoint                       | Mục đích                            |
|--------|--------------------------------|-------------------------------------|
| GET    | `/health`                      | Kiểm tra service                    |
| GET    | `/api/analytics/dashboard`     | Lấy dữ liệu dashboard tổng quan     |
| GET    | `/api/analytics/occupancy`     | Phân tích mật độ sử dụng không gian |
| GET    | `/api/analytics/trends`        | Xu hướng theo thời gian             |
| POST   | `/api/analytics/events`        | Nhận event dữ liệu từ service khác  |
| GET    | `/api/analytics/reports`       | Xuất báo cáo                        |

## 7. Phụ thuộc service khác

**Service này gọi đến:**
- PostgreSQL (lưu trữ dữ liệu lịch sử)
- Redis (cache & real-time data)
- Grafana (visualization)

**Service nào gọi đến service này:**
- Dashboard Frontend
- A6 - Core Business
- Admin Panel

## 8. Sơ đồ minh họa

```mermaid
flowchart LR
    subgraph External Services
        A1[A1 - IoT Ingestion]
        A2[A2 - Access Gate]
        A3[A3 - AI Vision]
    end

    subgraph Core
        Analytics[A5 - Analytics Service]
    end

    subgraph Storage
        DB[(PostgreSQL)]
        Cache[(Redis)]
    end

    subgraph Visualization
        Grafana[Grafana Dashboard]
        Dashboard[Dashboard Frontend]
    end

    Users[Users]

    A1 --> Analytics
    A2 --> Analytics
    A3 --> Analytics

    Analytics --> DB
    Analytics --> Cache
    Analytics --> Grafana
    Analytics --> Dashboard

    Dashboard --> Users