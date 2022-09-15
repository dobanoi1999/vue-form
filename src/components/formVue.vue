<template >
  <form class="row g-5" @submit.prevent="submitForm" autocomplete="off">
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
        <Input
          :inputProps="propertyList.cardNumber"
          :valueInput="formData.cardNumber"
          @update:valueInput="formatCardNumber"
          :errorMessage="v$?.cardNumber?.$errors[0]?.$message"
          @keyup="validateCardNumber"
          maxlength="19"
        />
      </div>
      <div class="p-3">
        <Input
          :inputProps="propertyList.cvn"
          :valueInput="formData.cvn"
          @update:valueInput="formData.cvn = $event"
          :errorMessage="v$?.cvn?.$errors[0]?.$message"
          maxlength="3"
        />
      </div>

      <div class="p-3">
        <Input
          :inputProps="propertyList.expiration"
          :valueInput="formData.expiration"
          @update:valueInput="formatExpiration"
          :errorMessage="v$?.expiration?.$errors[0]?.$message"
          maxlength="5"
        />
      </div>
    </div>
    <div class="col-12">
      <div class="p-3">
        <Range
          :rangeProps="propertyList.range"
          :valueRange="formData.donate"
          @update:valueRange="formData.donate = $event"
        />
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
import { required, email, helpers, minLength } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";

const initFormData = {
  firstName: "",
  lastName: "",
  company: "",
  email: "",
  phone: "",
  cvn: "",
  expiration: "",
  cardNumber: "",
  gender: "female",
  payment: "mastercard",
  donate: "1000",
};
const formLocal = localStorage.getItem("formValue");
console.log(JSON.parse(formLocal));
const formData = ref(formLocal ? JSON.parse(formLocal) : initFormData);

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
    cardNumber: {
      required: helpers.withMessage("Card number is required", required),
      minLengthValue: helpers.withMessage(
        "Card number is not valid",
        minLength(19)
      ),
    },
    cvn: {
      required: helpers.withMessage("CVN is required", required),
      minLengthValue: helpers.withMessage("CVN is not valid", minLength(3)),
    },
    expiration: {
      required: helpers.withMessage("Expiration is required", required),
      validExpiration: helpers.withMessage("Expiration is not valid", (e) => {
        if (e.length < 5) return false;
        const day = +e.slice(0, 2);

        if (day < 0 || day > 12) return false;
        return true;
      }),
      validateExpiration: helpers.withMessage("Card expired", (e) => {
        if (e.length < 5) return true;

        const currentYear = new Date().getFullYear().toString().slice(-2);
        const currentMoth = new Date().getMonth().toString();
        const year = +e.slice(-2);
        const moth = +e.slice(0, 2);
        if (year < +currentYear) return false;
        console.log(moth, +currentMoth);
        if (year === +currentYear && moth < +currentMoth) {
          return false;
        }
        return true;
      }),
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
  cardNumber: {
    label: "card number",
    id: "card_number",
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
  cvn: { label: "cvn", id: "cvn", required: true },
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
const formatCardNumber = (value) => {
  value = value.replace(/\W/gi, "").replace(/(.{4})/g, "$1 ");
  formData.value = { ...formData.value, cardNumber: value.trim() };
};

const validateCardNumber = (e) => {
  e.target.value = e.target.value.replace(/[^\d ]/g, "");
  return false;
};

const formatExpiration = (value) => {
  value = value
    .replace(
      /^([1-9]\/|[2-9])$/g,
      "0$1/" // 3 > 03/
    )
    .replace(
      /^(0[1-9]|1[0-2])$/g,
      "$1/" // 11 > 11/
    )
    .replace(
      /^([0-1])([3-9])$/g,
      "0$1/$2" // 13 > 01/3
    )
    .replace(
      /^(0?[1-9]|1[0-2])([0-9]{2})$/g,
      "$1/$2" // 141 > 01/41
    )
    .replace(
      /^([0]+)\/|[0]+$/g,
      "0" // 0/ > 0 and 00 > 0
    )
    .replace(
      /[^\d/]|^[/]*$/g,
      "" // To allow only digits and `/`
    )
    .replace(
      /\/\//g,
      "/" // Prevent entering more than 1 `/`
    );
  formData.value = { ...formData.value, expiration: value };
};

const submitForm = async () => {
  const rs = await v$.value.$validate();
  if (rs) {
    localStorage.setItem("formValue", JSON.stringify(formData.value));
    alert("Success submit form");
  }
};

const resetForm = () => {
  v$.value.$reset();
  formData.value = { ...initFormData };
};
</script>
   
<style>
</style>
   
   