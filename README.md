# go-common

## 云账户Demo

```go
// 接口配置信息
const (
	BrokerID     = "yiyun73"
	DealerID     = "12345678"
	AppKey       = "xxxxx"
	Des3Key      = "xxxxx"
	PrivateKey   = "xxxxx"
	YunPublicKey = "xxxxx"
	Gateway      = "https://api-jiesuan.yunzhanghu.com"
)

func main() {
	// 初始化SDK客户端
	client := sdk.New(BrokerID, DealerID, Gateway, AppKey, Des3Key, PrivateKey, YunPublicKey)

	// 1、银行卡下单,响应云账户综合服务平台订单流水号
	bankOrderParam := &sdk.BankOrderParam{
		CardNo:  "1111111111111111111111111",
		PhoneNo: "13333333333",
	}
	bankOrderParam.OrderID = "123456789012345"
	bankOrderParam.RealName = "张三"
	bankOrderParam.IDCard = "370829199101012219"
	bankOrderParam.Pay = "99.99"
	bankOrderParam.PayRemark = "银行卡打款"
	bankOrderParam.NotifyURL = "https://wwww.callback.com"

	ref, err := client.CreateBankOrder(bankOrderParam)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ref)

	// 2、支付宝下单,响应云账户综合服务平台订单流水号
	aliOrderParam := &sdk.AliOrderParam{
		CardNo:    "123456@ali.com",
		CheckName: "Check",
	}
	aliOrderParam.OrderID = "1234567890"
	aliOrderParam.RealName = "张三"
	aliOrderParam.IDCard = "370829199101012219"
	aliOrderParam.Pay = "99.99"
	aliOrderParam.PayRemark = "支付宝打款"
	aliOrderParam.NotifyURL = "https://wwww.callback.com"

	ref, err = client.CreateAliOrder(aliOrderParam)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ref)

	// 3、微信下单,响应云账户综合服务平台订单流水号
	wxOrderParam := &sdk.WxOrderParam{
		OpenID:    "aaabbbcccddd",
		Notes:     "微信打款备注",
		WxAppID:   "123456abcdefg",
		WxPayMode: "transfer",
	}
	wxOrderParam.OrderID = "1234567890"
	wxOrderParam.RealName = "张三"
	wxOrderParam.IDCard = "370829199101012219"
	wxOrderParam.Pay = "99.99"
	wxOrderParam.PayRemark = "微信打款"
	wxOrderParam.NotifyURL = "https://wwww.callback.com"

	ref, err = client.CereateWxOrder(wxOrderParam)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ref)

	// 4、订单回调,响应云账户综合服务平台订单打款回调订单信息
	orderDetails, err := client.OrderCallBack("data", "mess", "timestamp", "sign")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(orderDetails)

	// 5、查询订单信息,响应云账户综合服务平台订单信息
	orderInfo, err := client.QueryOrder("123456789012345", "银行卡", "")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(orderInfo)

	// 6、查询账户余额,响应商户在云账户综合服务平台账户余额信息
	accounts, err := client.QueryAccountBalance()
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(accounts)

	// 7、查询电子回单,响应商户打款订单的电子回单信息
	receiptFile, err := client.QueryReceiptFile("1234567890", ref)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(receiptFile)

	// 8、取消商户未打款订单,响应是否取消成功
	ok, err := client.CancelOrder("1234567890", ref, "银行卡")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 9、下载商户日订单文件,响应日订单文件下载地址
	url, err := client.DownloadOrderFile("2019-08-26")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(url)

	// 10、下载商户日流水文件,响应日流水文件下载地址
	url, err = client.DownloadBillFile("2019-08-26")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(url)

	// 11、查询商户充值记录,响应商户充值记录信息
	records, err := client.QueryRechargeRecord("2019-08-01", "2019-08-26")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(records)

	// 12、上传免验证用户信息,响应用户免验证信息是否上传成功
	userParam := &sdk.UserInfoParam{
		RealName:     "张三",
		IDCard:       "UFO1234567890",
		Birthday:     "19990909",
		CardType:     "passport",
		Country:      "CHN",
		Gender:       "男",
		Ref:          "USER-1234567890",
		NotifyURL:    "https://wwww.callback.com",
		UserImages:   []string{"front.jpg", "back.jpg"},
		CommentApply: "免验证用户",
	}

	ok, err = client.UploadUserInfo(userParam)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 13、免验证用户信息回调接口,响应云账户综合服务平台免验证名单回调通知信息
	userInfo, err := client.UserInfoCallback("data", "mess", "timestamp", "sign")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(userInfo)

	// 14、查询免验证名单是否存在,响应该云账户综合服务平台是否存在该免验证用户信息
	ok, err = client.CheckUserExist("张三", "UFO1234567890")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 15、查询发票信息,响应商户在云账户综合服务平台已开具发票金额和待开具发票金额
	invoice, err := client.QueryInvoice(2019)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(invoice)

	realName := "张三"
	idCard := "370829199101012219"
	cardNo := "1111111111"
	mobile := "13333333333"

	// 16、银行卡四要素鉴权发送短信上行接口,响应云账户综合服务平台银行卡四要素流水号
	ref, err = client.ElementVerifyRequest(idCard, realName, cardNo, mobile)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ref)

	// 17、银行卡四要素鉴权提交验证码确认接口,响应银行卡四要素校验是否通过
	ok, err = client.ElementVerifyConfirm(idCard, realName, cardNo, mobile, ref, "123456")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 18、银行卡四要素鉴权接口,响应银行卡四要素鉴权是否通过
	ok, err = client.Element4Check(idCard, realName, cardNo, mobile)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 19、银行卡三要素鉴权接口,响应银行卡三要素鉴权是否通过
	ok, err = client.Element3Check(idCard, realName, cardNo)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 20、实名制二要素鉴权接口,响应实名制二要素鉴权是否通过
	ok, err = client.IDCheck(idCard, realName)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(ok)

	// 21、查询银行卡信息接口,响应银行卡具体信息以及云账户综合服务平台是否支持该银行卡打款
	bankCardInfo, err := client.QueryBankCardInfo(cardNo, "中国人民银行")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(bankCardInfo)
}

```
