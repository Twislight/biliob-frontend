<template>
  <VLayout
    v-bind="binding"
    justify-center
  >
    <VFlex
      lg5
      md12
    >
      <VCard>
        <div>
          <VAlert
            :value="alertSuccess"
            type="success"
            transition="scale-transition"
          >恭喜你！注册成功！</VAlert>
          <VAlert
            :value="alertError"
            type="error"
            transition="scale-transition"
          >抱歉！{{ errorMsg }}!</VAlert>
        </div>

        <VCardTitle>
          <div>
            <h3 class="headline mb-1">注册</h3>
            <div>这是一个第三方网站，账号信息和B站并不通用。
              <br>登录后可以自主添加视频追踪，还能够实现关注UP主和收藏视频的功能（现在也许正在施工中、逃。
            </div>
          </div>
        </VCardTitle>
        <VCardActions>
          <VForm
            v-model="valid"
            style="width:100%"
          >
            <VTextField
              v-model="name"
              :rules="[rules.required]"
              browser-autocomplete="username"
              label="请输入用户名"
            />
            <VTextField
              v-model="password"
              :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show ? 'text' : 'password'"
              label="请输入密码"
              value
              browser-autocomplete="new-password"
              :rules="[rules.required]"
              hint="至少6个字符"
              @click:append="show = !show;"
            />
            <VTextField
              :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
              :type="show ? 'text' : 'password'"
              :rules="[rules.required, rules.min, rules.passwdMatch]"
              value
              name="input"
              label="请再输入一次密码呗"
              browser-autocomplete="new-password"
              hint="至少6个字符"
              @click:append="show = !show;"
            />
            <Center>
              <VBtn
                color="primary"
                :disabled="!valid"
                @click="submit"
              >注册</VBtn>
            </Center>
          </VForm>
        </VCardActions>
      </VCard>
    </VFlex>
  </VLayout>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      password: "",
      valid: false,
      show: null,
      alertSuccess: false,
      alertError: false,
      errorMsg: "",
      rules: {
        required: value => !!value || "求求你告诉我吧o(╥﹏╥)o",
        min: v => v.length >= 6 || '这么短小的话，也太不安全了吧Σ(⊙▽⊙"a',
        passwdMatch: v => {
          return v === this.password || "貌似两次输入不一致..手速太快了？罒ω罒";
        }
      }
    };
  },
  mounted() {
    this.$store.commit("toElse");
  },
  methods: {
    submit() {
      this.axios
        .post("/user", {
          name: this.name,
          password: this.password
        })
        .then(() => {
          this.alertSuccess = true;
          this.alertError = false;
          this.logined = true;
          setTimeout(() => {
            this.$router.push("/login");
          }, 2000);
        })
        .catch(error => {
          this.alertError = true;
          this.errorMsg = error.response.data.msg;
        });
    }
  }
};
</script>

<style>
.small-card {
  height: 100px;
}

.top-card {
  height: 212px;
}

.card-button {
  float: right;
}

.card-title {
  font-size: 18px;
}

.time {
  font-size: 13px;
}
</style>
