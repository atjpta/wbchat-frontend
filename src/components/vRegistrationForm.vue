<template>
<div>
    <div v-if="message" class="">
            <n-alert :title=message type="success" closable>
            </n-alert>
    </div>
    <div v-if="error" class="">
            <n-alert :title=error type="error" closable>
            </n-alert>
    </div>
    <Form
        @submit="registration"
        :validation-schema="userFormSchema"
        class="flex justify-center my-20 bg-gradient-to-l from-sky-100 to-sky-500/50 min-w-max max-w-xl rounded-3xl p-10 mx-auto text-lg"
    >
    <div>
        <div class="p-2">
            <div for="username" >Tài khoản : </div>
            <Field
                name="username"
                type="text"
                class="rounded-md bg-gray-800/10"
                v-model="username"
            />
            <div class="text-red-900">
                <ErrorMessage name="username" class="error-feedback" />
            </div>
        </div>
        <div class="p-2">
            <div for="name" >Tên : </div>
            <Field
                name="name"
                type="text"
                class="rounded-md bg-gray-800/10"
                v-model="name"
            />
            <div class="text-red-900">
                <ErrorMessage name="name" class="error-feedback" />
            </div>
        </div>
        <div class="p-2">
            <div for="email">E-mail : </div>
            <Field
                name="email"
                type="email"
                class="rounded-md bg-gray-800/10"
                v-model="email"
            />
            <div class="text-red-900">
                <ErrorMessage name="email" class="error-feedback" />
            </div>
        </div>
        <div class="p-2">
            <div for="password">Mật khẩu :</div>
            <Field
                name="password"
                type="password"
                class="rounded-md bg-gray-800/10"
                v-model="password"
            />
            <div class="text-red-900">
                <ErrorMessage name="password" class="error-feedback" />
            </div>
        </div>
        <div class="p-2">
            <div for="repassword">Nhập lại mật khẩu :</div>
            <Field
                name="repassword"
                type="password"
                class="rounded-md bg-gray-800/10"
            />
            <div class="text-red-900">
                <ErrorMessage name="repassword" class="error-feedback" />
            </div>
        </div>


        <div class="flex justify-center pt-10">
            <button 
              class="bg-teal-500 rounded-lg p-2 shadow-md w-44 hover:scale-125 duration-300"
              >
                Đăng kí
              </button>

        </div>
    </div>
    </Form>
</div>

</template>

<script>
import * as Yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import authService from "../services/auth.service";
export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    methods: {
        async registration(){
            this.message = "";
            this.error = "";
            try{
                await authService.create(
                {
                    username: this.username,
                    email: this.email,
                    password: this.password,
                    name : this.name,
                }
            );
                this.message = "Bạn đã đăng kí thành công";
            }
            catch(error){
                console.log(error);
                this.error = "Trùng user name hoặc mật khẩu!!";
            }
        }
    },
    data() {
        const userFormSchema = Yup.object().shape({
            username: Yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
            name: Yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
            email: Yup
                .string()
                .email("E-mail không đúng.")
                .required("E-mail phải có giá trị.")
                .max(50, "E-mail tối đa 50 ký tự."),
            password: Yup
                .string()
                .required("mật khẩu phải có giá trị.")
                .min(2, "mật phải ít nhất 2 ký tự."),
            repassword: Yup
                .string()
                .oneOf([Yup.ref('password'), null], "Mật khẩu không đúng!")             
                .required("mật khẩu phải có giá trị."),
        });
        return {
            // Chúng ta sẽ không muốn hiệu chỉnh props, nên tạo biến cục bộ
            //  để liên kết với các input trên form
            username: "",
            name :"",
            email: "",
            password: "",
            userFormSchema,
            message: "",
            error:"",
        };
    },
    
};
</script>