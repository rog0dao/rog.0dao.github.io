# 数据库ER图

使用Mermaid语法绘制的数据库实体关系图：

```mermaid
erDiagram
    %% 用户表
    t_user {
        BIGINT id PK
        VARCHAR student_id UK
        VARCHAR password
        VARCHAR name
        VARCHAR phone UK
        TINYINT role
        TINYINT status
        DATETIME create_time
        DATETIME update_time
    }

    %% 分类表
    categories {
        BIGINT id PK
        VARCHAR name
        VARCHAR description
        BIGINT parent_id FK
        INT sort_order
        TINYINT is_active
        DATETIME create_time
    }

    %% 设备表
    t_equipment {
        BIGINT id PK
        VARCHAR name
        VARCHAR category
        VARCHAR img_url
        DECIMAL original_price
        DECIMAL daily_rent
        DECIMAL deposit
        INT stock
        TINYINT status
        TEXT description
        DATETIME create_time
        DATETIME update_time
    }

    %% 商品表
    products {
        BIGINT id PK
        VARCHAR name
        VARCHAR description
        VARCHAR sku_code UK
        DECIMAL original_price
        DECIMAL daily_rent
        DECIMAL deposit_amount
        INT min_rent_days
        INT max_rent_days
        INT stock
        INT total_stock
        INT rented_count
        VARCHAR brand
        VARCHAR model
        VARCHAR specifications
        VARCHAR image_url
        VARCHAR detail_images
        INT warranty_period
        VARCHAR technical_specs
        VARCHAR power_requirements
        VARCHAR accessory_list
        VARCHAR compatibility
        INT status
        TINYINT is_hot
        TINYINT is_new
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
        DECIMAL daily_rent
        DECIMAL total_rent
        DECIMAL deposit
        DECIMAL actual_pay
        TINYINT order_status
        DATETIME pay_time
        DATETIME create_time
        DATETIME update_time
    }

    %% 租赁订单表
    rental_orders {
        BIGINT id PK
        VARCHAR order_no UK
        BIGINT user_id
        BIGINT product_id
        VARCHAR product_name
        DECIMAL daily_rent_snapshot
        DECIMAL deposit_snapshot
        DATE start_date
        DATE end_date
        DATE actual_return_date
        INT rental_days
        DECIMAL rent_amount
        DECIMAL deposit_amount
        DECIMAL refund_amount
        DECIMAL damage_fee
        DECIMAL overdue_fee
        DECIMAL cleaning_fee
        DECIMAL other_fee
        DECIMAL total_amount
        INT pay_status
        INT order_status
        VARCHAR payment_method
        DATETIME payment_time
        VARCHAR delivery_address
        VARCHAR receiver_name
        VARCHAR receiver_phone
        TEXT remark
        VARCHAR cancel_reason
        DATETIME create_time
        DATETIME update_time
    }

    %% 结算记录表
    t_settlement {
        BIGINT id PK
        BIGINT order_id FK
        INT overdue_days
        DECIMAL overdue_fee
        DECIMAL damage_fee
        DECIMAL total_deduct
        DECIMAL refund_deposit
        TINYINT settle_status
        BIGINT operator_id FK
        DATETIME settle_time
        DATETIME create_time
    }

    %% 押金结算表
    deposit_settlements {
        BIGINT id PK
        BIGINT order_id
        VARCHAR settlement_no UK
        DECIMAL original_deposit
        DECIMAL damage_fee
        INT overdue_days
        DECIMAL overdue_fee
        DECIMAL cleaning_fee
        DECIMAL other_fee
        DECIMAL total_deduction
        DECIMAL refund_amount
        DECIMAL actual_refund
        VARCHAR damage_description
        BIGINT inspector_id
        VARCHAR inspector_name
        DATETIME inspection_time
        INT settlement_status
        DATETIME settlement_time
        DATETIME refund_time
        VARCHAR remark
        DATETIME create_time
    }

    %% 损坏记录表
    damage_records {
        BIGINT id PK
        VARCHAR after_photo
        VARCHAR before_photo
        DECIMAL compensation_amount
        DECIMAL compensation_rate
        DATETIME create_time
        VARCHAR damage_type
        VARCHAR description
        VARCHAR inspector_notes
        DECIMAL repair_cost
        BIT repairable
        BIGINT settlement_id
        VARCHAR severity_level
    }

    %% 损坏赔偿配置表
    t_damage_config {
        BIGINT id PK
        VARCHAR damage_item
        DECIMAL compensate_fee
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
