<template >
  <div class="h-100 d-flex align-items-center justify-content-center">
    <form
      class="row gx-5 gy-4 form"
      @submit.prevent="submitForm"
      autocomplete="off"
    >
      <div class="col-6">
        <Input
          :inputProps="propertyList.firstName"
          :valueInput="formData.firstName"
          @update:valueInput="formData.firstName = $event.target.value"
          :errorMessage="v$?.firstName?.$errors[0]?.$message"
        />
      </div>
      <div class="col-6">
        <Select
          :selectProps="propertyList.gender"
          :selectValue="formData.gender"
          @update:selectValue="formData.gender = $event.target.value"
        />
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.lastName"
          :valueInput="formData.lastName"
          @update:valueInput="formData.lastName = $event.target.value"
          :errorMessage="v$?.lastName?.$errors[0]?.$message"
        />
      </div>
      <div class="col-6">
        <RadioGroup
          :radioGroupProps="propertyList.payment"
          :valueRadio="formData.payment"
          @update:valueRadio="formData.payment = $event.target.value"
          class="form-item-payment"
        />
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.company"
          :valueInput="formData.company"
          @update:valueInput="formData.company = $event.target.value"
          :errorMessage="v$?.company?.$errors[0]?.$message"
        />
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.cardNumber"
          :valueInput="formData.cardNumber"
          @update:valueInput="formatCardNumber"
          :errorMessage="v$?.cardNumber?.$errors[0]?.$message"
          @keyup="validateCardNumber"
          maxlength="19"
        />
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.email"
          :valueInput="formData.email"
          @update:valueInput="formData.email = $event.target.value"
          :errorMessage="v$?.email?.$errors[0]?.$message"
        />
      </div>
      <div class="col-6">
        <label for="expiration" class="form-label">
          expiration
          <span class="text-require">*</span>
        </label>
        <Datepicker
          id="expiration"
          v-model="formData.expiration"
          textInput
          monthPicker
          autoApply
          placeholder="MM/YY"
          format="MM/yy"
          inputClassName="form-control form-control-lg"
        ></Datepicker>
        <div>
          <span
            class="error-message"
            :class="v$?.expiration?.$errors[0]?.$message ? 'show-error' : ''"
          >
            {{ v$?.expiration?.$errors[0]?.$message }}
          </span>
        </div>
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.phone"
          :valueInput="formData.phone"
          @update:valueInput="formData.phone = $event.target.value"
          :errorMessage="v$?.phone?.$errors[0]?.$message"
        />
      </div>
      <div class="col-6">
        <Input
          :inputProps="propertyList.cvn"
          :valueInput="formData.cvn"
          @update:valueInput="formData.cvn = $event.target.value"
          :errorMessage="v$?.cvn?.$errors[0]?.$message"
          maxlength="3"
        />
      </div>

      <div class="col-12 py-4">
        <Range
          :rangeProps="propertyList.range"
          :valueRange="formData.donate"
          @update:valueRange="formData.donate = $event"
        />
      </div>
      <div class="col-12">
        <div class="d-flex justify-content-end">
          <div class="row gx-4">
            <div class="col">
              <button
                type="submit"
                class="btn btn-success btn-lg py-3 px-5 text-uppercase"
              >
                Submit
              </button>
            </div>
            <div class="col">
              <button
                type="button"
                @click="resetForm"
                class="
                  btn btn-outline-secondary btn-lg
                  py-3
                  px-5
                  text-uppercase
                "
              >
                Reset
              </button>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>
   
<script setup>
import Input from "./input.vue";
import Select from "./select.vue";
import RadioGroup from "./radioGroup.vue";
import Range from "./range.vue";
import Datepicker from "@vuepic/vue-datepicker";

import { computed, ref } from "vue";
import { required, email, helpers, minLength } from "@vuelidate/validators";
import useVuelidate from "@vuelidate/core";

import "@vuepic/vue-datepicker/dist/main.css";

const initFormData = {
  firstName: "",
  lastName: "",
  company: "",
  email: "",
  phone: "",
  cvn: "",
  expiration: null,
  cardNumber: "",
  gender: "female",
  payment: "mastercard",
  donate: "1000",
};
const formLocal = localStorage.getItem("formValue");

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
      phoneValid: helpers.withMessage("Phone number is not valid", (value) => {
        return /(84|0[3|5|7|8|9])+([0-9]{8})\b/g.test(value);
      }),
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
      validateExpiration: helpers.withMessage("Card expired", (e) => {
        const currentYear = new Date().getFullYear();
        const currentMoth = new Date().getMonth();
        const year = e.year;
        const month = e.month + 1;
        if (year < currentYear) return false;

        if (year === currentYear && month < currentMoth) {
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
    required: true,
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
.btn {
  font-weight: 700;
  font-size: 14px;
}
.form-item-payment {
  height: 100%;
}
.form {
  max-width: 80%;
}
input[type="month"] {
  visibility: hidden;
  position: absolute;
  bottom: 0;
}
.dp__input {
  background-color: unset;
  font-family: unset;
  transition: unset;
  line-height: unset;
  color: unset;
  padding: 0.5rem 1rem;
  font-size: 1.25rem;
  border-radius: 0.5rem;
  padding-left: 35px;
}
</style>
   
   