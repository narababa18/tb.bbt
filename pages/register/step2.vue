<template>
  <div>
    <v-app-bar color="primary" dense flat dark>
      <v-toolbar-title>ลงทะเบียน</v-toolbar-title>
    </v-app-bar>
    <v-container class="pt-0 pb-0">
      <v-row>
        <v-col cols="12">
          <div class="mt-8 text-primary text-title text-center">
            Step 2 of 2
          </div>
        </v-col>
        <v-col cols="12">
          <v-form>
            <p class="text-center text-main mb-0 mt-0">รายละเอียดเพิ่มเติม</p>
            <div class="gender-group d-flex mt-3">
              <p>เพศ</p>
              <div
                class="circle circle-1"
                :class="form.gender == 1 ? 'active' : ''"
                @click="chooseGender(1)"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-gender-female"
                  viewBox="0 0 16 16"
                >
                  <path
                    fill-rule="evenodd"
                    d="M8 1a4 4 0 1 0 0 8 4 4 0 0 0 0-8zM3 5a5 5 0 1 1 5.5 4.975V12h2a.5.5 0 0 1 0 1h-2v2.5a.5.5 0 0 1-1 0V13h-2a.5.5 0 0 1 0-1h2V9.975A5 5 0 0 1 3 5z"
                  />
                </svg>
              </div>
              <p>ชาย</p>
              <div
                class="circle circle-2"
                :class="form.gender == 2 ? 'active' : ''"
                @click="chooseGender(2)"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-gender-male"
                  viewBox="0 0 16 16"
                >
                  <path
                    fill-rule="evenodd"
                    d="M9.5 2a.5.5 0 0 1 0-1h5a.5.5 0 0 1 .5.5v5a.5.5 0 0 1-1 0V2.707L9.871 6.836a5 5 0 1 1-.707-.707L13.293 2H9.5zM6 6a4 4 0 1 0 0 8 4 4 0 0 0 0-8z"
                  />
                </svg>
              </div>
              <p>หญิง</p>
            </div>
              <v-dialog
                ref="dialog"
                v-model="modal"
                :return-value.sync="date"
                persistent
                width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="form.hbdate"
                    label="วันเกิด (ปี ค.ศ./เดือน/วัน)"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="form.hbdate" scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="modal = false">
                    Cancel
                  </v-btn>
                  <v-btn text color="primary" @click="$refs.dialog.save(date)">
                    OK
                  </v-btn>
                </v-date-picker>
              </v-dialog>
            <v-text-field
              v-model="form.phone"
              dense
              label="เบอร์โทร"
            ></v-text-field>
            <p class="text-center text-main mb-0 mt-3">
              ช่องทางติดต่ออื่นกรณีฉุกเฉิน
            </p>
            <v-text-field v-model="form.etc1" dense></v-text-field>
            <v-text-field v-model="form.etc2" dense></v-text-field>
            <v-btn
              rounded
              color="primary"
              dark
              class="w-100 mt-2 my-btn-3"
              @click="register"
              >ลงทะเบียน</v-btn
            >
            <v-btn
              rounded
              color="secondary"
              dark
              class="w-100 mt-1 my-btn-2"
              @click="back"
              >กลับ</v-btn
            >
          </v-form>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  data() {
    return {
      form: {
        gender: this.$store.getters.getRegister.gender,
        hbdate: this.$store.getters.getRegister.hbdate,
        phone: this.$store.getters.getRegister.phone,
        etc1: this.$store.getters.getRegister.etc1,
        etc2: this.$store.getters.getRegister.etc2,
      },
      date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
      menu: false,
      modal: false,
        methods: {
    chooseGender(gender) {
      this.form.gender = gender;
    }
   }
    }

  },

    back() {
      this.$router.push('/register')
    },
    register() {
      this.$store.dispatch('setRegister', this.form)
      this.$axios.patch('https://tb-bbt-default-rtdb.asia-southeast1.firebasedatabase.app/members/Line:001/profile.json', this.$store.getters.getRegister).then((res) => {
        this.$router.push('/register/done')
      })
      }
    }
  
</script>

<style lang="scss" scoped>
.v-form {
  padding: 0 10px;
  margin: 0 10px;
}
.gender-group {
  p {
    margin-bottom: 0;
    align-self: center;
    margin-right: 20px;
  }
  .circle-1 {
    width: 45px;
    height: 45px;
    color: #fff;
    border-radius: 50%;
    position: relative;
    background: rgba($color: #000000, $alpha: 0.3);
    margin-right: 7px;
    &.active {
      background: #1a56be;
    }
    svg {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  }
  .circle-2 {
    width: 45px;
    height: 45px;
    color: #fff;
    border-radius: 50%;
    position: relative;
    background: rgba($color: #000000, $alpha: 0.3);
    margin-right: 7px;
    &.active {
      background: #ec3298;
    }
    svg {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  }
}
</style>