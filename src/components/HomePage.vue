<template>
  <v-container>
    <v-row class="d-flex justify-center px-9">
      <v-col cols="12" sm="11" md="10" lg="8" class="body px-15">
        <v-col cols="12" class="d-flex pb-0 px-0">
          <v-col cols="4" sm="3" class="d-flex flex-column pb-0 pl-0 pr-5">
            <div :class="['mb-2', errorDay ? 'tag2' : 'tag1']">DAY</div>
            <div :class="[errorDay ? 'input10' : 'input1']">
              <input
                type="number"
                class="input-one px-0 px-sm-4 py-3"
                v-model="day"
                placeholder="DD"
              />
            </div>
            <div v-if="errorDay" class="mt-2 error">{{ errorDay }}</div>
          </v-col>
          <v-col cols="4" sm="3" class="d-flex flex-column pb-0 pl-0 pr-5">
            <div :class="['mb-2', errorMonth ? 'tag2' : 'tag1']">MONTH</div>
            <div :class="[errorMonth ? 'input20' : 'input2']">
              <input
                type="number"
                class="input-two px-0 px-sm-4 py-3"
                v-model="month"
                placeholder="MM"
              />
            </div>
            <div v-if="errorMonth" class="mt-2 error">{{ errorMonth }}</div>
          </v-col>
          <v-col cols="4" sm="3" class="d-flex flex-column pb-0 pl-0 pr-5">
            <div :class="['mb-2', errorYear ? 'tag2' : 'tag1']">YEAR</div>
            <div :class="[errorYear ? 'input30' : 'input3']">
              <input
                type="number"
                class="input-three px-0 px-sm-4 py-3"
                v-model="year"
                placeholder="YYYY"
              />
            </div>
            <div v-if="errorYear" class="mt-2 error">{{ errorYear }}</div>
          </v-col>
        </v-col>
        <div @click="calculateAge" class="line-container mb-10 mt-sm-12 mt-16">
          <div class="circle d-flex justify-center align-center">
            <img class="img2" src="/src/assets/icon-arrow.svg" alt="" />
          </div>
        </div>

        <div class="result">
          <v-col class="d-flex flex-column">
            <div class="ryears d-flex text-black py-0">
              <div v-if="!age" class="dash">--</div>
              <div v-else class="res1">{{ age.years }}</div>
              <div class="text1 ml-1">years</div>
            </div>
            <div class="rmonths d-flex text-black">
              <div v-if="!age" class="dash">--</div>
              <div v-else class="res2">{{ age.months }}</div>
              <div class="text2 ml-1">months</div>
            </div>
            <div class="rdays d-flex text-black">
              <div v-if="!age" class="dash">--</div>
              <div v-else class="res3">{{ age.days }}</div>
              <div class="text3 ml-1">days</div>
            </div>
          </v-col>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const day = ref("");
const month = ref("");
const year = ref("");
const age = ref(null);

// Separate error messages for day, month, and year
const errorDay = ref("");
const errorMonth = ref("");
const errorYear = ref("");

const calculateAge = () => {
  // Clear all previous error messages and reset age
  errorDay.value = "";
  errorMonth.value = "";
  errorYear.value = "";
  age.value = null;

  // Convert inputs to numbers
  const dayNum = parseInt(day.value);
  const monthNum = parseInt(month.value);
  const yearNum = parseInt(year.value);

  // Check for empty fields and set appropriate error messages
  let hasError = false;
  if (!day.value) {
    errorDay.value = "This field is required";
    hasError = true;
  }
  if (!month.value) {
    errorMonth.value = "This field is required";
    hasError = true;
  }
  if (!year.value) {
    errorYear.value = "This field is required";
    hasError = true;
  }

  // Validate the date inputs
  if (!isValidDate(dayNum, monthNum, yearNum)) {
    return; // If any input is invalid, stop further execution
  }
  if (hasError) return; // If any field is missing, stop here

  // Create date objects for calculations
  const dob = new Date(yearNum, monthNum - 1, dayNum);
  const today = new Date();

  // Calculate age components (years, months, days)
  let years = today.getFullYear() - dob.getFullYear();
  let months = today.getMonth() - dob.getMonth();
  let days = today.getDate() - dob.getDate();

  // Adjust months and days if necessary
  if (days < 0) {
    months--;
    days += new Date(today.getFullYear(), today.getMonth(), 0).getDate();
  }
  if (months < 0) {
    years--;
    months += 12;
  }

  age.value = { years, months, days };
};

const isValidDate = (day, month, year) => {
  const today = new Date();
  const currentYear = today.getFullYear();

  let isValid = true;

  // Check year validity
  if (year > currentYear) {
    errorYear.value = "Must be a valid year";
    isValid = false;
  }

  // Check month validity
  if (month < 1 || month > 12) {
    errorMonth.value = "Must be a valid month";
    isValid = false;
  }

  // Check day validity
  if (day < 1 || day > 31) {
    errorDay.value = "Must be a valid day";
    isValid = false;
  }

  // Check for valid days in the given month
  const daysInMonth = new Date(year, month, 0).getDate();
  if (day > daysInMonth) {
    errorDay.value = `Must be a valid day.`;
    isValid = false;
  }

  return isValid;
};
</script>

 
<style scoped>
@font-face {
  font-family: f1;
  src: url(/src/assets/Poppins-ExtraBoldItalic.ttf);
}
@font-face {
  font-family: f2;
  src: url(/src/assets/Poppins-Bold.ttf);
}
@font-face {
  font-family: f3;
  src: url(/src/assets/Poppins-Italic.ttf);
}
.body {
  background-color: #fff;
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 150px;
  border-top-right-radius: 20px;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Input wrapper styles */
.input1,
.input2,
.input3 {
  border: 1.5px solid hsl(0, 0%, 93%);
  border-radius: 5px;
  padding-left: 5px; /* Add padding for better focus visibility */
}
.input10,
.input20,
.input30 {
  border: 1.5px solid hsl(0, 100%, 67%);
  border-radius: 5px;
  padding: 5px; /* Add padding for better focus visibility */
}
.input1:hover,
.input2:hover,
.input3:hover {
  border: 1.5px solid hsl(260, 28%, 64%);
}
.input-one,
.input-two,
.input-three {
  color: black;
  font-family: f2;
  font-size: 27px;
  width: 100%;
}
.input1:focus-within,
.input2:focus-within,
.input3:focus-within {
  border-color: hsl(260, 28%, 64%); /* Change to your desired color */
  /* box-shadow: 0 0 5px hsl(259, 100%, 75%); */
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.input-one:focus,
.input-two:focus,
.input-three:focus {
  outline: none; /* Remove default focus outline */
}

.line {
  width: 100%;
  height: 2px;
  background-color: hsl(0, 0%, 93%);
}
.img1 {
  background-color: hsl(258, 99%, 65%);
  width: 102px; /* Control the circle size here */
  height: 87px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1;
}
.img1 img {
  width: 50%;
}
.dash {
  font-family: f1;
  color: hsl(259, 100%, 65%);
  font-size: 100px;
  letter-spacing: 15px;
}
.res1,
.res2,
.res3 {
  font-family: f1;
  color: hsl(259, 100%, 65%);
  font-size: 100px;
  letter-spacing: 0px;
}

.text1,
.text2,
.text3 {
  color: black;
  font-size: 100px;
  font-family: f1;
}
.ryears {
  margin-top: -35px;
}
.rmonths {
  margin-top: -42px;
  margin-bottom: -37px;
}
.tag1 {
  color: hsl(0, 1%, 44%);
  font-family: f2;
  font-size: 12px;
  letter-spacing: 3px;
}
.tag2 {
  color: hsl(0, 100%, 67%);
  font-family: f2;
  font-size: 12px;
  letter-spacing: 3px;
}
.input-one::placeholder,
.input-two::placeholder,
.input-three::placeholder {
  color: hsl(0, 1%, 44%); /* Change to your desired color */
  opacity: 1; /* Ensures the color applies consistently */
  font-family: f2; /* Optional: Apply a custom font */
  font-size: 22px; /* Optional: Adjust font size */
}
.error {
  color: hsl(0, 100%, 67%);
  font-family: f3;
  font-size: 11px;
}
.line-container {
  position: relative;
  width: 100%;
  height: 1px; /* Thickness of the line */
  background-color: hsl(0, 0%, 86%); /* Line color */
  cursor: pointer;
}

.circle {
  position: absolute;
  top: 50%; /* Align vertically */
  transform: translateY(-50%); /* Center on the y-axis */
  width: 80px;
  height: 80px;
  background-color: hsl(258, 100%, 65%);
  border-radius: 50%; /* Make it a circle */
  transition: all 0.3s ease; /* Smooth transition */
}

/* Default: Circle at the right end for screens >= 599.5px */
.circle {
  right: 0; /* Position the circle at the right end */
}
@media (min-width: 960px) and (max-width: 1279.5px) {
  .dash {
    font-size: 80px;
    letter-spacing: 10px;
  }
  .text1,
  .text2,
  .text3 {
    font-size: 80px;
  }
}
@media (min-width: 600px) and (max-width: 959.5px) {
  .dash {
    font-size: 80px;
    letter-spacing: 10px;
  }
  .text1,
  .text2,
  .text3 {
    font-size: 80px;
  }
  .input10,
  .input20,
  .input30 {
    padding: 0px; /* Add padding for better focus visibility */
  }
  .input1,
  .input2,
  .input3 {
    padding-left: 0px; /* Add padding for better focus visibility */
  }
}
@media (max-width: 599.5px) {
  .circle {
    top: 50%; /* Align vertically */
    transform: translateY(-50%); /* Center on the y-axis */
    width: 60px;
    height: 60px;
  }
  .dash {
    font-size: 40px;
    letter-spacing: 10px;
  }
  .text1,
  .text2,
  .text3 {
    font-size: 40px;
  }
  .ryears {
    margin-top: 0px;
  }
  .rmonths {
    margin-top: -10px;
    margin-bottom: -10px;
  }
  .line-part {
    margin-top: 40px;
    margin-bottom: 30px;
  }
  .res1,
  .res2,
  .res3 {
    font-family: f1;
    color: hsl(259, 100%, 65%);
    font-size: 45px;
    letter-spacing: 0px;
  }
  .img1 {
    width: 98px; /* Control the circle size here */
    height: 75px;
  }
  .input-one,
  .input-two,
  .input-three {
    font-size: 24px;
  }
  .input-one::placeholder,
  .input-two::placeholder,
  .input-three::placeholder {
    padding-left: 5px;
  }
  .circle {
    left: 50%; /* Center horizontally */
    transform: translate(-50%, -50%); /* Fully center on both axes */
  }
  .img2
  {
    transform: scale(0.6);
  }
}
</style>