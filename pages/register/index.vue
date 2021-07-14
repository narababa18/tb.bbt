<template>
    <div>
        <v-app-bar 
        color="primary" 
        dense 
        flat 
        dark
        >
            <v-toolbar-title>ลงทะเบียน</v-toolbar-title>
        </v-app-bar>
        <v-container class="pt-0 pb-0">
            <v-row>
                <v-col cols="12">
                    <div class="mt-8 text-primary text-title text-center">
                    Step 1 of 2
                    </div>
                </v-col>
                <v-col cols="12" class="text-center pb-0">
                    <img v-if="getLine.pictureUrl ==''" src="~/assets/profile.png" alt="" width="155" >
                    <img v-else :src="getLine.pictureUrl" alt="" width="155" >
                </v-col>
                <v-col cols="12" class="text-center pt-2 pb-0">
                    {{ getLine.displayName }}
                </v-col>
                <v-col cols="12">
                    <v-form>
                        <v-text-field
                        v-model="form.firstname"
                        dense
                        label="ชื่อ"
                        ></v-text-field>
                        <v-text-field
                        v-model="form.lastname"
                        dense
                        label="นามสกุล"
                        ></v-text-field>
                        <v-text-field
                        v-model="form.hospnumb"
                        dense
                        label="หมายเลขประจำตัวผู้ป่วย(HN)"
                        ></v-text-field>
                        <v-btn rounded color="primary" dark class="w-100 mt-16 my-btn-1" @click="next">ถัดไป</v-btn>
                    </v-form>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>
<script>
export default {
    mounted(){
        liff.init({
            liffId: '1656184696-vxQY5zeg'
        }).then(() => {
            if(liff.isLoggedIn()){
                liff.getProfile().then(profile => {
                this.$store.dispatch('setLine', profile)
                })
            }else{
                liff.login();
            }
        })
    },
    computed: {
        getLine(){
            return this.$store.getters.getLine;
        }
    },
    data() {
        return{
            form: {
                firstname: this.$store.getters.getRegister.firstname,
                lastname: this.$store.getters.getRegister.lastname,
                hospnumb: this.$store.getters.getRegister.hospnumb,
            }
        }
    },
    methods: {
        next(){
            this.$store.dispatch('setRegister', this.form)
            this.$router.push('/register/step2')
        }
    }
}

</script>
<style lang="scss" scoped>
.v-form{
    padding: 0 10px;
    margin: 0 10px;

}

</style>