# 数据库ER图

使用Mermaid语法绘制的数据库实体关系图：

```mermaid
erDiagram
    %% 用户表
    t_user {
        BIGINT id PK
        VARCHAR(20) student_id UK
        VARCHAR(64) password
        VARCHAR(30) name
        VARCHAR(11) phone UK
        TINYINT role
        TINYINT status
        DATETIME create_time
        DATETIME update_time
    }

    %% 分类表
    categories {
        BIGINT id PK
        VARCHAR(255) name
        VARCHAR(500) description
        BIGINT parent_id FK
        INT sort_order
        TINYINT(1) is_active
        DATETIME create_time
    }

    %% 设备表
    t_equipment {
        BIGINT id PK
        VARCHAR(50) name
        VARCHAR(30) category
        VARCHAR(255) img_url
        DECIMAL(10,2) original_price
        DECIMAL(8,2) daily_rent
        DECIMAL(10,2) deposit
        INT stock
        TINYINT status
        TEXT description
        DATETIME create_time
        DATETIME update_time
    }

    %% 商品表
    products {
        BIGINT id PK
        VARCHAR(255) name
        VARCHAR(1000) description
        VARCHAR(255) sku_code UK
        DECIMAL(10,2) original_price
        DECIMAL(10,2) daily_rent
        DECIMAL(10,2) deposit_amount
        INT min_rent_days
        INT max_rent_days
        INT stock
        INT total_stock
        INT rented_count
        VARCHAR(255) brand
        VARCHAR(255) model
        VARCHAR(255) specifications
        VARCHAR(255) image_url
        VARCHAR(255) detail_images
        INT warranty_period
        VARCHAR(255) technical_specs
        VARCHAR(255) power_requirements
        VARCHAR(255) accessory_list
        VARCHAR(255) compatibility
        INT status
        TINYINT(1) is_hot
        TINYINT(1) is_new
        DATETIME create_time
        DATETIME update_time
        BIGINT category_id FK
    }

    %% 订单表
    t_order {
        BIGINT id PK
        BIGINT user_id FK
        BIGINT equipment_id FK
        INT rent_days
        DATETIME start_time
        DATETIME end_time
        DECIMAL(8,2) daily_rent
        DECIMAL(10,2) total_rent
        DECIMAL(10,2) deposit
        DECIMAL(10,2) actual_pay
        TINYINT order_status
        DATETIME pay_time
        DATETIME create_time
        DATETIME update_time
    }

    %% 租赁订单表
    rental_orders {
        BIGINT id PK
        VARCHAR(32) order_no UK
        BIGINT user_id
        BIGINT product_id
        VARCHAR(100) product_name
        DECIMAL(10,2) daily_rent_snapshot
        DECIMAL(10,2) deposit_snapshot
        DATE start_date
        DATE end_date
        DATE actual_return_date
        INT rental_days
        DECIMAL(10,2) rent_amount
        DECIMAL(10,2) deposit_amount
        DECIMAL(10,2) refund_amount
        DECIMAL(10,2) damage_fee
        DECIMAL(10,2) overdue_fee
        DECIMAL(10,2) cleaning_fee
        DECIMAL(10,2) other_fee
        DECIMAL(10,2) total_amount
        INT pay_status
        INT order_status
        VARCHAR(20) payment_method
        DATETIME(6) payment_time
        VARCHAR(200) delivery_address
        VARCHAR(50) receiver_name
        VARCHAR(20) receiver_phone
        TEXT remark
        VARCHAR(200) cancel_reason
        DATETIME(6) create_time
        DATETIME(6) update_time
    }

    %% 结算记录表
    t_settlement {
        BIGINT id PK
        BIGINT order_id FK
        INT overdue_days
        DECIMAL(8,2) overdue_fee
        DECIMAL(10,2) damage_fee
        DECIMAL(10,2) total_deduct
        DECIMAL(10,2) refund_deposit
        TINYINT settle_status
        BIGINT operator_id FK
        DATETIME settle_time
        DATETIME create_time
    }

    %% 押金结算表
    deposit_settlements {
        BIGINT id PK
        BIGINT order_id
        VARCHAR(32) settlement_no UK
        DECIMAL(10,2) original_deposit
        DECIMAL(10,2) damage_fee
        INT overdue_days
        DECIMAL(10,2) overdue_fee
        DECIMAL(10,2) cleaning_fee
        DECIMAL(10,2) other_fee
        DECIMAL(10,2) total_deduction
        DECIMAL(10,2) refund_amount
        DECIMAL(10,2) actual_refund
        VARCHAR(2000) damage_description
        BIGINT inspector_id
        VARCHAR(255) inspector_name
        DATETIME(6) inspection_time
        INT settlement_status
        DATETIME(6) settlement_time
        DATETIME(6) refund_time
        VARCHAR(255) remark
        DATETIME(6) create_time
    }

    %% 损坏记录表
    damage_records {
        BIGINT id PK
        VARCHAR(255) after_photo
        VARCHAR(255) before_photo
        DECIMAL(10,2) compensation_amount
        DECIMAL(5,2) compensation_rate
        DATETIME(6) create_time
        VARCHAR(255) damage_type
        VARCHAR(255) description
        VARCHAR(255) inspector_notes
        DECIMAL(10,2) repair_cost
        BIT(1) repairable
        BIGINT settlement_id
        VARCHAR(255) severity_level
    }

    %% 损坏赔偿配置表
    t_damage_config {
        BIGINT id PK
        VARCHAR(50) damage_item
        DECIMAL(10,2) compensate_fee
        TINYINT status
        DATETIME update_time
    }

    %% 系统参数配置表
    t_sys_config {
        BIGINT id PK
        VARCHAR param_key UK
        VARCHAR param_value
        VARCHAR remark
        DATETIME update_time
    }

    %% 外键关系
    categories ||--o{ categories : parent_id
    products }o--|| categories : category_id
    t_order }o--|| t_user : user_id
    t_order }o--|| t_equipment : equipment_id
    t_settlement }o--|| t_order : order_id
    t_settlement }o--|| t_user : operator_id
    deposit_settlements }o--|| rental_orders : order_id
    damage_records }o--|| deposit_settlements : settlement_id
```

## 如何查看此ER图

1. **使用VSCode**：安装Mermaid插件后，打开此文件即可看到可视化的ER图。
2. **使用GitHub**：将此文件上传到GitHub仓库，GitHub会自动渲染Mermaid图表。
3. **使用Mermaid在线编辑器**：访问 https://mermaid.live/，复制粘贴上述代码即可查看。

## 关系说明

- **用户表(t_user)** 与 **订单表(t_order)**：一对多关系，一个用户可以创建多个订单
- **设备表(t_equipment)** 与 **订单表(t_order)**：一对多关系，一个设备可以被多个订单租赁
- **订单表(t_order)** 与 **结算记录表(t_settlement)**：一对多关系，一个订单对应一个结算记录
- **用户表(t_user)** 与 **结算记录表(t_settlement)**：一对多关系，一个管理员可以处理多个结算
- **分类表(categories)** 与 **分类表(categories)**：自关联，支持多级分类
- **分类表(categories)** 与 **商品表(products)**：一对多关系，一个分类可以包含多个商品
- **租赁订单表(rental_orders)** 与 **押金结算表(deposit_settlements)**：一对多关系，一个租赁订单对应一个押金结算
- **押金结算表(deposit_settlements)** 与 **损坏记录表(damage_records)**：一对多关系，一个押金结算可以包含多个损坏记录
