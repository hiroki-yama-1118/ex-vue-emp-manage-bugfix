<template>
  <div class="container">
    <div class="row login-page">
      <div class="col s12 z-depth-6 card-panel">
        <form class="login-form">
          <div class="row"></div>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">mail_outline</i>
              <div v-show="errorLogin" class="error">
                {{ errorLoginCom }}
              </div>
              <div v-show="errorMailAddress" class="error">
                {{ errorMailAddressCom }}
              </div>
              <input
                class="validate"
                id="mailAddress"
                type="email"
                v-model="mailAddress"
              />
              <label for="mailAddress" data-error="wrong" data-success="right"
                >メールアドレス</label
              >
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix">lock_outline</i>
              <div v-show="errorPassWord" class="error">
                {{ errorPassWordCom }}
              </div>
              <input id="password" type="password" v-model="password" />
              <label for="password">パスワード</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <button
                class="btn btn-register waves-effect waves-light col s12"
                type="button"
                v-on:click="loginAdmin"
              >
                ログイン
              </button>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s6 m6 l6">
              <p class="margin medium-small">
                <router-link to="/registerAdmin"
                  >管理者登録はこちら</router-link
                >
              </p>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import config from "@/const/const";
import axios from "axios";

/**
 * ログインをする画面.
 */
@Component
export default class LoginAdmin extends Vue {
  // メールアドレス
  private mailAddress = "";
  // パスワード
  private password = "";
  //メールアドレスのエラー
  private errorMailAddress = false;
  //メールアドレスのエラーメッセージ
  private errorMailAddressCom = "";
  //パスワードのエラー
  private errorPassWord = false;
  //パスワードのエラーメッセージ
  private errorPassWordCom = "";
  //ログインのエラー
  private errorLogin = false;
  //ログインのエラーメッセージ
  private errorLoginCom = "";

  /**
   * ログインする.
   *
   * @remarks
   * 本メソッドは非同期でWebAPIを呼び出しログインをするためasync/await axiosを利用しています。これらを利用する場合は明示的に戻り値にPromiseオブジェクト型を指定する必要があります。
   * @returns Promiseオブジェクト
   */
  async loginAdmin(): Promise<void> {
    const response = await axios.post(`${config.EMP_WEBAPI_URL}/login`, {
      mailAddress: this.mailAddress,
      password: this.password,
    });
    console.dir("response:" + JSON.stringify(response));

    // 従業員一覧に遷移する
    if (response.data.status !== "error") {
      this.$router.push("/employeeList");
    }
    if (response.data.status == "error") {
      this.errorMailAddress = true;
      this.errorMailAddressCom = "メールアドレスを入力してください";
      this.errorPassWord = true;
      this.errorPassWordCom = "パスワードを入力してください";
      this.errorLogin = true;
      this.errorLoginCom = "ログインできませんでした";
    } else if (this.mailAddress !== "") {
      this.errorMailAddress = false;
    } else if (this.password !== "") {
      this.errorMailAddress = false;
    }
  }
}
</script>

<style scoped>
.login-page {
  width: 600px;
}
.error {
  color: red;
  text-align: center;
}
</style>
