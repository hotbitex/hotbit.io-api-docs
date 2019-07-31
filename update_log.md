# 更新日志

2019.3.28

对订单增加了status字段用于表示订单状态标志
当(status &(与) 0x8)为真的时候表示当前订单是取消的

2019.5.9

明确接口里面涉及的时间单位为秒

2019.5.10

修改market.deals接口参数传输错误提示，此接口不需要签名认证

2019.6.3

修改order.put_limit接口，增加了is_fee字段，用于标记是否需要使用抵扣折扣币，1-表示使用抵扣折扣币，0-表示不使用

订单详情增加了3个字段fee_stock，alt_fee, deal_fee_alt。

fee_stock:抵扣币的名称

alt_fee:抵扣币的优惠折扣

deal_fee_alt:已经抵扣的抵扣币

修改了status字段状态，增加了与0x80取真的判断代表是否使用了抵扣币，（status&0x80）TRUE-使用了抵扣币，FALSE-未使用抵扣币

2019.7.31

更正market.user_deals接口的错误
