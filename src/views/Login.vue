<template>
  <div class="login">
    <van-nav-bar
      left-text="返回"
      right-text="先逛一逛"
      @click-right="goState('Home')"
    >
      <template #left>
        <div class="nav-left">
          <div class="logo">
            <img class="img" src="../assets/images/coffe.png" alt="" />
          </div>
          <div class="logo-text">Fruit Coffee</div>
        </div>
      </template>
    </van-nav-bar>

    <div class="user-box">
      <div class="bg-box"></div>
      <div class="welcome-text">
        <p>果粒咖啡商城</p>
        <p>欢迎您登录！</p>
      </div>
      <div class="user-content">
        <van-form>
          <van-field
            v-model="userInfo.phone"
            name="手机号"
            label="手机号"
            placeholder="手机号"
            autocomplete="off"
          />

          <van-field
            v-model="userInfo.password"
            :type="isLoginPassword ? 'password' : 'text'"
            name="密码"
            label="密码"
            placeholder="密码必须为字母开头"
            :right-icon="isLoginPassword ? 'closed-eye' : 'eye-o'"
            autocomplete="off"
            @click-right-icon="toggleLoginPasswordType"
          />
          <div class="forgot">
            <span @click="goState('Forgot')">忘记密码?</span>
          </div>

          <div class="commit-btn">
            <van-button round block color="#006699" @click="login">
              登录
            </van-button>
          </div>
          <div class="commit-btn">
            <van-button
              round
              block
              type="default"
              color="#3294c5"
              @click="toggleRegisterBox"
            >
              注册
            </van-button>
          </div>
          <div class="tips">点击登录代表您已同意<span>用户协议</span>和<span>隐私政策</span></div>
        </van-form>
      </div>
    </div>

    <!-- 注册 -->
    <van-popup
      class="register-box"
      v-model="isShow"
      position="bottom"
      closeable
      round
    >
      <div class="register-title">注册</div>
      <van-form>
        <van-field
          v-model="userRegisterInfo.phone"
          name="手机号"
          label="手机号"
          placeholder="11位手机号"
          autocomplete="off"
        />

        <van-field
          v-model="userRegisterInfo.password"
          :type="isRegisterPassword ? 'password' : 'text'"
          name="密码"
          label="密码"
          placeholder="密码必须为字母开头"
          :right-icon="isRegisterPassword ? 'closed-eye' : 'eye-o'"
          autocomplete="off"
          @click-right-icon="toggleRegsiterPasswordType"
        />

        <van-field
          v-model="userRegisterInfo.nickName"
          name="昵称"
          label="昵称"
          placeholder="昵称"
          autocomplete="off"
        />

        <div class="commit-btn register-btn">
          <van-button round block color="#3294c5" @click="register">
            注册
          </van-button>
        </div>
      </van-form>
    </van-popup>
  </div>
</template>

<script>
//导入外部样式表
import '../assets/less/login.less'

//导入表单验证模块
import { validForm } from '../assets/js/validForm'

export default {
  name: 'Login',
  data() {

    return {
      //用户登录信息
      userInfo: {
        phone: '',
        password: ''
      },

      //用户注册信息
      userRegisterInfo: {
        phone: '',
        password: '',
        nickName: ''
      },

      //是否显示注册框
      isShow: false,

      //切换注册密码框的类型
      isRegisterPassword: true,

      // 切换登录密码框的类型
      isLoginPassword: true
    };
  },

  methods: {
    //切换注册框
    toggleRegisterBox() {
      this.isShow = true;
    },

    //切换注册密码框的类型
    toggleRegsiterPasswordType() {
      this.isRegisterPassword = !this.isRegisterPassword;
    },

    //切换密码密码框的类型
    toggleLoginPasswordType() {
      this.isLoginPassword = !this.isLoginPassword;
    },

    //注册
    register() {
      //构造表单验证信息
      let o = {
        phone: {
          value: this.userRegisterInfo.phone,
          errorMsg: '手机号格式不正确',
          reg: /^1[3-9]\d{9}$/
        },
        password: {
          value: this.userRegisterInfo.password,
          errorMsg: '字母数字_组合且字母开头(6-16位)',
          reg: /^[A-Za-z]\w{5,15}$/
        },
        nickName: {
          value: this.userRegisterInfo.nickName,
          errorMsg: '字母数字_汉字组合(1-10位)',
          reg: /^[\w\u4e00-\u9fa5]{1,10}$/
        },
      };

      let isPass = validForm.valid(o);

      if (isPass) {

        console.log('this.appkey ==> ', this.appkey);

        //赋值用户注册信息
        let userInfo = Object.assign({}, this.userRegisterInfo);
        userInfo.appkey = this.appkey;
        console.log('userInfo ==> ', userInfo);
        console.log('this.userRegisterInfo ==> ', this.userRegisterInfo);

        //启动加载提示
        this.$toast.loading({
          //文本提示
          message: '加载中...',
          //禁止穿透点击
          forbidClick: true,
          //提示时间, 0: 没有时间限制
          duration: 0
        });

        //发起注册请求
        this.axios({
          //请求类型
          method: 'POST',
          //请求路径
          url: '/register',

          //POST请求参数, object
          data: userInfo

        }).then(result => {
          //关闭加载提示
          this.$toast.clear();

          if (result.data.code == 100) {
            this.isShow = false;
          } else {
            //如果注册失败，手机被注册了
            this.$toast(result.data.msg);
          }

          // console.log('result ==> ', result);
        }).catch(err => {
          //关闭加载提示
          this.$toast.clear();

          console.log('err ==> ', err);
        })

        this.userRegisterInfo.phone = ''
        this.userRegisterInfo.password = ''
        this.userRegisterInfo.nickName = ''

      }
    },

    // 登录
    login() {
      //构造表单验证信息
      let o = {
        phone: {
          value: this.userInfo.phone,
          errorMsg: '手机号格式不正确',
          reg: /^1[3-9]\d{9}$/
        },
        password: {
          value: this.userInfo.password,
          errorMsg: '密码格式不正确',
          reg: /^[A-Za-z]\w{5,15}$/
        }
      };

      let isPass = validForm.valid(o);
      if (isPass) {
        // 发起登录请求
        // 复制用户注册信息

        let userInfo = Object.assign({}, this.userInfo);
        userInfo.appkey = this.appkey;

        //启动加载提示
        this.$toast.loading({
          //文本提示
          message: '加载中...',
          //禁止穿透点击
          forbidClick: true,
          //提示时间, 0: 没有时间限制
          duration: 0
        });

        //发起注册请求
        this.axios({
          //请求类型
          method: 'POST',
          //请求路径
          url: '/login',

          //POST请求参数, object
          data: userInfo

        }).then(result => {
          this.$toast.clear();

          console.log('result ==> ', result);

          if (result.data.code == 200) {
            //登录成功
            //保存token, 以便后面验证登录
            localStorage.setItem('__tk', result.data.token);

            //其他操作
            this.$router.push({ name: 'Home' })
          } else {
            this.$toast(result.data.msg);
          }

        }).catch(err => {
          this.$toast.clear();

          console.log('err ==> ', err);
        })

        this.userInfo.phone = ''
        this.userInfo.password = ''

      }
    },
    //跳转到首页
    goState(name) {
      this.$router.push({ name });
    }


  }
}
</script>