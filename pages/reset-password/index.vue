<template>
  <div class="p-32 flex items-center justify-around">
    <div class="p-10 bg-sec-dark rounded-3xl">
      <h1 class="text-3xl text-center">
        Set a new password
      </h1>
      <h6 class="text-sm opacity-50">Choose a new password for your account</h6>
      <div class="flex flex-col mt-10">
        <label for="password" class="opacity-75 capitalize">
          password
        </label>
        <input
          type="password"
          class="bg-dark p-3 my-2 rounded-xl placeholder-white placeholder-opacity-25 outline-none text-white w-full text-sm"
          placeholder="enter your new password"
          id="password"
          v-model="password"
        />
      </div>
      <div class="flex flex-col mt-3">
        <label for="password" class="opacity-75 capitalize">
          password confirm
        </label>
        <input
          type="password"
          class="bg-dark p-3 my-2 rounded-xl placeholder-white placeholder-opacity-25 outline-none text-white w-full text-sm"
          placeholder="enter your password confirm"
          id="password"
          v-model="password_confirm"
        />
      </div>
      <button
        type="submit"
        class="w-full py-2 font-medium text-xl bg-green my-5 rounded-2xl"
        style="outline: none"
        @click="checkForm()"
      >
        reset
      </button>
    </div>
    <div class="flex flex-col items-center">
      <h1 class="text-4xl relative z-10">
        Your <strong class="text-green">Safety</strong> is one of our priority
      </h1>
      <img src="~/assets/images/group15.svg" alt="" class="transform -translate-y-12 relative z-0" />
    </div>
  </div>
</template>

<script>
import EventService from "@/services/EventService";
export default {
  data() {
    return {
      password: "",
      password_confirm: ""
    };
  },
  methods: {
    checkForm() {
      if (this.password.length < 8) {
        this.$swal.fire({
          title: "Error!",
          text: "Password of at least 8 characters.",
          icon: "error",
          confirmButtonText: "ok",
          showCloseButton: true
        });
      } else if (
        !this.password.match(
          /^(((?=.*[a-z])(?=.*[A-Z]))|((?=.*[a-z])(?=.*[0-9]))|((?=.*[A-Z])(?=.*[0-9])))(?=.{6,})/
        )
      ) {
        this.$swal.fire({
          title: "Error!",
          text:
            "Password should contains number, uppercase and lowercase letter.",
          icon: "error",
          confirmButtonText: "ok",
          showCloseButton: true
        });
      } else if (this.password != this.password_confirm) {
        this.$swal.fire({
          title: "Error!",
          text: "confirm passwrod not matched.",
          icon: "error",
          confirmButtonText: "ok",
          showCloseButton: true
        });
      } else {
        this.resetPassword();
      }
    },
    resetPassword() {
      const route = this.$route;
      const { user_id, timestamp, signature } = route.query;
      EventService.resetPassword(
        user_id,
        +timestamp,
        signature,
        this.password,
        this.password_confirm
      )
        .then(res => {
          this.$swal.fire({
            title: "password reset successfully!",
            text: "now login to your account for continue.",
            icon: "success",
            confirmButtonText: "ok",
            showCloseButton: true
          });
          this.$router.push("/login");
        })
        .catch(err => {
          console.log(err);
          this.$swal.fire({
            title: "Error!",
            text: "an error occurred.",
            icon: "error",
            confirmButtonText: "ok",
            showCloseButton: true
          });
        });
    }
  }
};
</script>

<style lang="scss" scoped></style>
