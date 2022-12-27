<script setup>
import { useField, useForm } from "vee-validate";
import { toFormValidator } from "@vee-validate/zod";
import * as zod from "zod";
import { watch } from "vue";
const validationSchema = toFormValidator(
  zod.object({
    username: zod
      .string({
        required_error: "Username is required",
        invalid_type_error: "Username must be a string",
      })
      .nonempty({ message: "Can't be empty" })
      .min(6, { message: "Too short" }),
    password: zod
      .string({
        required_error: "Password is required",
        invalid_type_error: "Name must be a string",
      })
      .nonempty({ message: "Can't be empty" })
      .min(8, { message: "Too short" }),
  })
);

const { handleSubmit, errors, isSubmitting, resetForm } = useForm({
  validationSchema,
});

const { value: username, meta: usernameMeta } = useField("username");
const { value: password, meta: passwordMeta } = useField("password");

const onSubmit = handleSubmit((values) => {
  console.log(JSON.stringify(values));
  console.log("errors", errors.value);
  if (isSubmitting) {
    console.log("is Submitting");
  }

  resetForm();
});

watch(
  () => errors.value,
  () => {
    console.log("errors", errors.value);
  }
);
</script>

<template>
  <div class="flex justify-center items-center h-screen bg-gray-300 px-6">
    <div class="p-6 max-w-sm w-full bg-white shadow-md rounded-md">
      <div class="flex justify-center items-center">
        <span class="text-gray-700 font-semibold text-2xl">LOGIN</span>
      </div>
      <form class="mt-4" method="POST" @submit.prevent="onSubmit">
        <label class="block">
          <span class="text-gray-700 text-sm">Username</span>
          <input
            type="text"
            name="username"
            class="form-input mt-1 block w-full rounded-md focus:outline-none"
            placeholder="Fill Username"
            v-model="username"
          />
          <div
            v-if="errors.username"
            class="inline-flex max-w-sm w-full bg-red-200 shadow-sm rounded-md overflow-hidden mt-2"
          >
            <div class="px-4 py-2">
              <p class="text-gray-600 text-sm">{{ errors.username }}</p>
            </div>
          </div>

          <div
            v-if="usernameMeta.valid"
            class="inline-flex max-w-sm w-full bg-green-200 shadow-sm rounded-md overflow-hidden mt-2"
          >
            <div class="px-4 py-2">
              <p class="text-gray-600 text-sm">Ok</p>
            </div>
          </div>
        </label>

        <label class="block mt-3">
          <span class="text-gray-700 text-sm">Password</span>
          <input
            type="password"
            v-model="password"
            name="password"
            class="form-input mt-1 block w-full rounded-md"
            placeholder="Fill Password"
          />
          <div
            v-if="errors.password"
            class="inline-flex max-w-sm w-full bg-red-200 shadow-sm rounded-md overflow-hidden mt-2"
          >
            <div class="px-4 py-2">
              <p class="text-gray-600 text-sm">{{ errors.password }}</p>
            </div>
          </div>

          <div
            v-if="passwordMeta.valid"
            class="inline-flex max-w-sm w-full bg-green-200 shadow-sm rounded-md overflow-hidden mt-2"
          >
            <div class="px-4 py-2">
              <p class="text-gray-600 text-sm">Ok</p>
            </div>
          </div>
        </label>

        <div class="mt-6">
          <button
            type="submit"
            class="py-2 px-4 text-center bg-red-600 rounded-md w-full text-white text-sm hover:bg-red-500 focus:outline-none"
          >
            Submit
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
