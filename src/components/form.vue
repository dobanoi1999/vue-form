<template>
  <form class="row g-5">
    <div class="col-6">
      <Input
        :inputProps="{
          label: 'First Name',
          id: 'firstName',
          required: true,
        }"
      />
      <Input
        :inputProps="{
          label: 'Last Name',
          id: 'lastName',
          required: true,
        }"
      />
      <Input
        :inputProps="{ label: 'Company', id: 'company', required: true }"
      />
      <Input :inputProps="{ label: 'Email', id: 'email', required: true }" />

      <Input
        :inputProps="{
          label: 'phone number',
          id: 'phone',
          required: true,
        }"
      />
    </div>
    <div class="col-6">
      <Select
        :selectProps="{
          label: 'gender',
          id: 'male',
          required: true,
          options: [
            { key: 1, value: 'male', name: 'Male' },
            { key: 1, value: 'female', name: 'Female' },
            { key: 1, value: 'other', name: 'other' },
          ],
        }"
      />
      <RadioGroup
        :radioGroupProps="{
          label: 'Payment mode',

          required: true,
          radios: [
            { value: 'master', name: 'MasterCard' },
            { value: 'visa', name: 'Visa' },
            { value: 'Amex', name: 'Amex' },
          ],
        }"
      />
      <Input :inputProps="{ label: 'cvn', id: 'cvn', required: true }" />
      <Input
        :inputProps="{
          label: 'expiration',
          id: 'expiration',
          required: true,
        }"
      />
      <Range
        :rangeProps="{
          label: 'expiration',
          id: 'expiration',
          required: true,
        }"
      />
    </div>
  </form>
</template>
   
<script>
import Input from "./input.vue";
import Select from "./select.vue";
import RadioGroup from "./radioGroup.vue";
import Range from "./range.vue";

export default {
  name: "Form-vue",
  components: { Input, Select, RadioGroup, Range },
};
</script>
   
<style>
</style>
   
   