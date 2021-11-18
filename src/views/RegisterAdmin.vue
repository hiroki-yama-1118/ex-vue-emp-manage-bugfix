<template>
  <div class="container">
    <div class="row register-page">
      <form class="col s12" id="reg-form">
        <div class="error">{{ errorMessage }}</div>
        <div class="row">
          <div class="input-field col s6">
            <div class="error">
              {{ errorLastNameMessageCom }}
            </div>
            <input
              id="last_name"
              type="text"
              class="validate"
              v-model="lastName"
              required
            />
            <label for="last_name">姓</label>
          </div>
          <div class="input-field col s6">
            <div class="error">
              {{ errorLastNameMessageCom }}
            </div>
            <input
              id="first_name"
              type="text"
              class="validate"
              v-model="firstName"
              required
            />
            <label for="first_name">名</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <div class="error">
              {{ errorMailAddressCom }}
            </div>
            <input
              id="email"
              type="email"
              class="validate"
              v-model="mailAddress"
              required
            />
            <label for="email">メールアドレス</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <div class="error">
              {{ errorPasswordCom }}
            </div>
            <input
              id="password"
              type="password"
              class="validate"
              minlength="8"
              v-model="password"
              required
            />
            <label for="password">パスワード</label>
          </div>
        </div>

        <div class="input-field col s12">
          <div class="error">
            {{ errorRePasswordCom }}
          </div>
          <input
            id="repassword"
            type="password"
            class="validate"
            minlength="8"
            v-model="repassword"
            required
          />
          <label for="password">確認用パスワード</label>
        </div>

        <div class="row">
          <div class="input-field col s6">
            <button
              class="btn btn-large btn-register waves-effect waves-light"
              type="button"
              v-on:click="registerAdmin"
            >
              登録
              <i class="material-icons right">done</i>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import config from "@/const/const";
import axios from "axios";

/**
 * 管理者登録をする画面.
 */
@Component
export default class RegisterAdmin extends Vue {
  // 姓
  private lastName = "";
  // 名
  private firstName = "";
  // メールアドレス
  private mailAddress = "";
  // パスワード
  private password = "";
  // パスワード
  private repassword = "";
  //姓のエラーメッセージ
  private errorLastNameMessageCom = "";
  //メールアドレスのエラーメッセージ
  private errorMailAddressCom = "";
  //パスワードのエラーメッセージ
  private errorPasswordCom = "";
  //確認用パスワードのエラーメッセージ
  private errorRePasswordCom = "";
  //ログインのエラーメッセージ
  private errorMessage = "";

  /**
   * 管理者情報を登録する.
   *
   * @remarks
   * 本メソッドは非同期でWebAPIを呼び出し管理者登録をするためasync/await axiosを利用しています。これらを利用する場合は明示的に戻り値にPromiseオブジェクト型を指定する必要があります。
   * @returns Promiseオブジェクト
   */
  async registerAdmin(): Promise<void> {
    let hasError = false;

    if (this.lastName === "" || this.firstName === "") {
      this.errorLastNameMessageCom = "姓または名を入力してください";
      hasError = true;
    }
    if (this.mailAddress === "") {
      this.errorMailAddressCom = "アドレスを入力してください";
      hasError = true;
    }
    if (this.password === "") {
      this.errorPasswordCom = "パスワードを入力してください";
      hasError = true;
    } else if (this.password !== this.repassword) {
      this.errorRePasswordCom = "パスワードが不一致です";
      hasError = true;
    }
    if (hasError == true) {
      return;
    }
    this.$router.push("/employeeList");

    // 管理者登録処理
    const response = await axios.post(`${config.EMP_WEBAPI_URL}/insert`, {
      name: this.lastName + " " + this.firstName,
      mailAddress: this.mailAddress,
      password: this.password,
    });
    console.dir("response:" + JSON.stringify(response));

    if (response.data.status === "error") {
      this.errorMessage = "登録できませんでした";
    } else {
      this.$router.push("/loginAdmin");
    }
  }
}
</script>

<style scoped>
.register-page {
  width: 600px;
}
.error {
  color: red;
  text-align: center;
}
</style>
