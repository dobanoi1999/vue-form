<template>
  <form class="row g-5" @submit.prevent="submitForm(formData)">
    <div class="col-6">
      <div class="p-3">
        <Input
          :inputProps="propertyList.firstName"
          :valueInput="formData.firstName"
          @update:valueInput="formData.firstName = $event"
          :errorMessage="v$?.firstName?.$errors[0]?.$message"
        />
        {{ v$._value?.email.$error }}
      </div>
      <div class="p-3">
        <Input
          :inputProps="propertyList.lastName"
          :valueInput="formData.lastName"
          @update:valueInput="formData.lastName = $event"
          :errorMessage="v$?.lastName?.$errors[0]?.$message"
        />
      </div>
      <div class="p-3">
        <Input
          :inputProps="propertyList.company"
          :valueInput="formData.company"
          @update:valueInput="formData.company = $event"
          :errorMessage="v$?.company?.$errors[0]?.$message"
        />
      </div>
      <div class="p-3">
        <Input
          :inputProps="propertyList.email"
          :valueInput="formData.email"
          @update:valueInput="formData.email = $event"
          :errorMessage="v$?.email?.$errors[0]?.$message"
        />
      </div>
      <div class="p-3">
        <Input
          :inputProps="propertyList.phone"
          :valueInput="formData.phone"
          @update:valueInput="formData.phone = $event"
          :errorMessage="v$?.phone?.$errors[0]?.$message"
        />
      </div>
    </div>
    <div class="col-6">
      <div class="p-3">
        <Select
          :selectProps="propertyList.gender"
          :selectValue="formData.gender"
          @update:selectValue="formData.gender = $event"
        />
      </div>
      <div class="p-3">
        <RadioGroup
          :radioGroupProps="propertyList.payment"
          :valueRadio="formData.payment"
          @update:valueRadio="formData.payment = $event"
        />
      </div>
      <div class="p-3">
        <Input :inputProps="propertyList.cvn" />
      </div>

      <div class="p-3">
        <Input :inputProps="propertyList.expiration" />
      </div>
    </div>
    <div class="col-12">
      <div class="p-3">
        <Range :rangeProps="propertyList.range" />
      </div>
    </div>
    <div class="col-12">
      <div class="d-flex justify-content-end">
        <div class="p-2">
          <button type="submit" class="btn btn-primary px-3 py-2">
            Submit
          </button>
        </div>
        <div class="p-2">
          <button
            type="button"
            @click="resetForm"
            class="btn btn-outline-secondary px-3 py-2"
          >
            Reset
          </button>
        </div>
      </div>
    </div>
  </form>
</template>
   
<script setup>
import Input from "./input.vue";
import Select from "./select.vue";
import RadioGroup from "./radioGroup.vue";
import Range from "./range.vue";
import { computed, ref } from "vue";
import { required, email, helpers } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";

const initFormData = {
  firstName: "",
  lastName: "",
  company: "",
  email: "",
  phone: "",
  cvn: "",
  expiration: "",
  gender: "female",
  payment: "mastercard",
};
const formData = ref(initFormData);

const rules = computed(() => {
  return {
    firstName: {
      required: helpers.withMessage("First name is required", required),
    },

    lastName: {
      required: helpers.withMessage("Last name is required", required),
    },
    company: { required: helpers.withMessage("Company is required", required) },
    email: {
      required: helpers.withMessage("Email is required", required),
      email,
    },
    phone: {
      required: helpers.withMessage("Phone number is required", required),
    },
    cvn: { required: helpers.withMessage("CVN is required", required) },
    expiration: {
      required: helpers.withMessage("Expiration is required", required),
    },
  };
});
const v$ = useVuelidate(rules, formData);

const propertyList = {
  firstName: {
    label: "First Name",
    id: "firstName",
    required: true,
  },
  lastName: {
    label: "Last Name",
    id: "lastName",
    required: true,
  },
  company: { label: "Company", id: "company", required: true, type: "text" },
  email: { label: "Email", id: "email", required: true, type: "text" },
  phone: {
    label: "phone number",
    id: "phone",
    required: true,
  },
  gender: {
    label: "gender",
    id: "male",
    required: true,
    options: [
      { key: 1, value: "male", name: "Male" },
      { key: 1, value: "female", name: "Female" },
      { key: 1, value: "other", name: "Other" },
    ],
  },
  payment: {
    label: "Payment mode",
    required: true,
    radios: [
      { value: "mastercard", name: "MasterCard" },
      { value: "visa", name: "Visa" },
      { value: "amex", name: "Amex" },
    ],
  },
  cvn: { label: "cvn", id: "cvn", required: true, type: "number" },
  expiration: {
    label: "expiration",
    id: "expiration",
    required: true,
  },
  range: {
    label: "Donate us",
    id: "Donate",
  },
};

const submitForm = async (x) => {
  console.log(x);
};

const resetForm = () => {
  v$.value.$reset();
  formData.value = { ...initFormData };
};
</script>
   
<style>
</style>
   
   