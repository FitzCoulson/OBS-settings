# 前期准备 #
* 首先下载一个Chrome内核的浏览器，Chrome或Edge浏览器等
* Chrome：https://www.google.cn/intl/zh-CN/chrome/
* Edge:https://www.microsoft.com/zh-cn/edge?r=1

# 打开小红书直播链接接口 #
* 链接：https://robs.xiaohongshu.com/

# 打开Console #
* 按F12打开浏览器开发人员工具，Chrome选择Console，Edge选择控制台

# 复制代码 #
* 点击代码块右上角复制到Console中
```
class LittleRedBookHelper {
  countryCode = '86'
  apiHost = 'robs.xiaohongshu.com'
  deviceIdKeyName = 'device-id'
  userInfoKeyName = 'user-info'
  defaultTitleKeyName = 'default-title'
  defaultCoverKeyName = 'default-cover'

  deviceId
  getDeviceId() {
    var deviceId = localStorage.getItem(this.deviceIdKeyName)
    if (deviceId) {
      return deviceId
    } else {
      var randomCode = Math.random().toString('32').substring(2)
      deviceId = ''
      var codeLen = 11
      for (var i = 0; i <= codeLen; i++) {
        deviceId += `${randomCode[i % codeLen]}${
          i % 2 === 1 && i !== codeLen ? ':' : ''
        }`
      }

      localStorage.setItem(this.deviceIdKeyName, deviceId)
      return deviceId
    }
  }

  checkHost() {
    var isInRightPage = window.location.host === this.apiHost
    if (!isInRightPage) {
      var rightHref = `https://${this.apiHost}`
      if (
        confirm(
          `当前页面非小红书站点,代码无法正常运行\n点击[确定]跳转至\n${rightHref}\n并且手动打开控制台重新执行代码?`,
        )
      ) {
        location.href = rightHref
      }
      throw 'wrong page,process terminated!'
    }
  }

  constructor() {
    this.checkHost()
    this.deviceId = this.getDeviceId()
  }

  verificationSmsCode(phone, callBack) {
    var self = this
    var smsCode = prompt('请输入小红书短信验证码')
    while (isNaN(parseInt(smsCode)) || smsCode.length !== 6) {
      var msg = '验证码需为6位数字!'
      alert(msg)
      smsCode = prompt('请输入小红书短信验证码')
    }
    fetch(
      `/api/sns/login_by_sms?phone_number=${phone}&phone_country=${this.countryCode}&sms_code=${smsCode}`,
      {
        method: 'post',
        headers: {
          'device-id': self.deviceId,
        },
      },
    ).then((r) =>
      r.json().then((resp) => {
        if (resp && resp.data && resp.data.access_token) {
          localStorage.setItem(
            self.userInfoKeyName,
            JSON.stringify({
              phone: phone,
              expireTime:
                new Date().getTime() + resp.data.expire_seconds * 1000,
              access_token: resp.data.access_token,
            }),
          )
          console.log('登录成功!')
          callBack()
        } else {
          console.log(resp)
          alert('登录失败请刷新重试!')
        }
      }),
    )
  }

  login(phoneNumber, callBack) {
    var self = this

    fetch(
      `/api/sns/send_sms?phone_number=${phoneNumber}&phone_country=${this.countryCode}`,
      {
        method: 'post',
        headers: {
          'device-id': self.deviceId,
        },
      },
    ).then((r) => {
      r.json().then((resp) => {
        if (resp.success && resp.result == 0) {
          this.verificationSmsCode(phoneNumber, callBack)
        } else {
          console.log(resp)
        }
      })
    })
  }

  reLogin(callBack) {
    var self = this
    var phoneInput = prompt(
      '登录信息已过期,需要输入手机号以登录获取小红书直播推流码',
    )
    self.login(phoneInput, () => {
      callBack()
    })
  }

  getObsPathAndStartLive() {
    var self = this
    var userInfo = JSON.parse(localStorage.getItem(self.userInfoKeyName))

    if (userInfo && userInfo.expireTime > new Date().getTime()) {
      fetch('/api/sns/live/pre', {
        headers: {
          'device-id': self.deviceId,
          sid: userInfo.access_token,
        },
      }).then((r) => {
        r.json().then((resp) => {
          if (!resp.success) {
            console.log(resp)
            self.reLogin(() => {
              self.getObsPathAndStartLive()
            })
          }
          var pushUrl = resp.data.url.push_url
          var id = pushUrl.split('live/')[1].split('?')[0]
          var defaultTitle =
            localStorage.getItem(self.defaultTitleKeyName) || '开始直播辣~!'
          var defaultCover =
            localStorage.getItem(self.defaultCoverKeyName) ||
            'https://ci.xiaohongshu.com/010219016o4yr657xvm0116ncgt0leon4z?imageView2/2/w/1080/format/jpg'
          var title = prompt('请输入直播标题', defaultTitle)
          var cover = prompt('请输入直播封面', defaultCover)
          localStorage.setItem(self.defaultTitleKeyName, title)
          localStorage.setItem(self.defaultCoverKeyName, cover)

          fetch(`/api/sns/live/${id}/start`, {
            method: 'post',
            headers: {
              'device-id': self.deviceId,
              sid: userInfo.access_token,
            },
            body: JSON.stringify({
              name: title,
              notice: '',
              is_distribute: true,
              cover: cover,
              lesson_id: 0,
            }),
          }).then((r) => {
            console.table({
              提示: '直播已开始,请尽快接入推流码!',
              推流码地址: pushUrl,
            })
          })
        })
      })
    } else {
      self.reLogin(() => {
        self.getObsPathAndStartLive()
      })
    }
  }
}

new LittleRedBookHelper().getObsPathAndStartLive()
```

# 运行 #
* 按回车运行后首次登录会提示输入手机号验证码，即可继续后续步骤，之后登录将不再需要验证码
* 输入手机号
* 输入短信验证码
* 输入直播间标题
* 输入封面地址
* 此时即可获得推流码地址
* 服务器地址填
* ![image](https://user-images.githubusercontent.com/42920883/161201015-ad15ddf6-fdeb-41ab-bec5-35fd21c7bc3e.png)
* 串流密钥填
* ![image](https://user-images.githubusercontent.com/42920883/161201066-1eb9d239-3b23-4f31-a52f-282449b32116.png)

