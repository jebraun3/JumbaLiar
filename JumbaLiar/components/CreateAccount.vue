<script setup lang="ts">
import { activeTab } from "~/app.vue";
enum error {
  NONE,
  BLANK,
  INVALID,
}
const email = ref("");
const username = ref("");
const password = ref("");
const confirmPassword = ref("");
const emailFlag = ref(error.NONE);
const usernameFlag = ref(error.NONE);
const lastEmail = ref("");
const lastUsername = ref("");

function getEmailStatus() {
  if (lastEmail.value == email.value && emailFlag.value != error.NONE)
    return "p-invalid";
  return true;
}

function getUsernameStatus() {
  if (lastUsername.value == username.value && usernameFlag.value)
    return "p-invalid";
  return true;
}

function getPasswordStatus() {
  if (password.value != confirmPassword.value) return "p-invalid";
}

async function create() {
  let check = false;
  // Checks that the email is a valid email (I think, who knows with regex)
  let regex = new RegExp(
    "([-!#-'*+/-9=?A-Z^-~]+(\.[-!#-'*+/-9=?A-Z^-~]+)*|'([]!#-[^-~ \t]|(\\[\t -~]))+')@[0-9A-Za-z]([0-9A-Za-z-]{0,61}[0-9A-Za-z])?(\.[0-9A-Za-z]([0-9A-Za-z-]{0,61}[0-9A-Za-z])?)+"
  );
  if (email.value == "") {
    emailFlag.value = error.BLANK;
    lastEmail.value = email.value;
    check = true;
  } else if (!regex.test(email.value)) {
    emailFlag.value = error.INVALID;
    lastEmail.value = email.value;
    check = true;
  }
  // TODO: await check if username exists
  if (username)
    if (password.value != confirmPassword.value)
      // Checks passwords match
      return;

  // TODO: await backend create account call
  if (!check) activeTab.setActiveTab("Login");
}

function cancel() {
  activeTab.setActiveTab("Login");
}
</script>

<template>
  <div class="mainContainer">
    <div class="loginContainer">
      <Card unstyled class="card">
        <template #header>
          <p class="title">Create an Account</p>
        </template>
        <template #content>
          <div class="contentContainer">
            <img />
            <InputText
              v-model="email"
              placeholder="Email"
              :class="getEmailStatus()"
            />
            <small
              v-if="getEmailStatus() == 'p-invalid'"
              class="warning"
              id="email-help"
            >
              Invalid email address!
            </small>
            <InputText
              class="spacer"
              v-model="username"
              placeholder="Username"
              :class="getUsernameStatus()"
            />
            <small
              v-if="getUsernameStatus() == 'p-invalid'"
              class="warning"
              id="username-help"
            >
              Username is taken!
            </small>
            <!-- :feedback="false" -->
            <Password
              class="spacer"
              v-model="password"
              placeholder="Password"
              toggleMask
            />
            <Password
              class="spacer"
              :feedback="false"
              v-model="confirmPassword"
              placeholder="Confirm Password"
              toggleMask
              :class="getPasswordStatus()"
            />
            <small
              v-if="getPasswordStatus() == 'p-invalid'"
              class="warning"
              id="password-help"
            >
              Password must match!
            </small>
            <div class="controlContainer spacer">
              <Button
                class="grey button"
                label="Cancel"
                icon="pi pi-times"
                @click="cancel()"
              />
              <Button
                class="orange button"
                label="Create Account"
                icon="pi pi-check"
                @click="create()"
              />
            </div>
          </div>
        </template>
      </Card>
    </div>
  </div>
</template>

<style scoped>
.mainContainer {
  display: flex;
  width: 100%;
  height: 100%;
}
.loginContainer {
  margin: auto;
}
.card {
  border-radius: var(--card-radius);
  padding: var(--main-content-gap);
  gap: var(--main-content-gap);
  border-radius: var(--card-radius);
  background: white;
}
.title {
  text-align: center;
  font-size: 32px;
  font-weight: 700;
  color: black;
}
.spacer {
  margin-top: 20px;
}
.contentContainer {
  display: flex;
  flex-direction: column;
  /* gap: 20px; */
}
.controlContainer {
  gap: 15px;
  display: flex;
}
.button {
  height: 45px;
  border: 0px;
  flex-grow: 1;
}
.warning {
  padding-left: 8px;
}
.grey {
  background-color: var(--sidebar-icon-grey);
}
.orange {
  background-color: var(--sidebar-highlight);
}
:deep(.p-divider) {
  margin: 0px;
}
:deep(.p-inputtext) {
  width: 315px;
  border-inline-width: 0px;
  border-top-width: 0px;
  border-bottom-width: 1px;
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  border-color: #5b5b5b;
}
</style>
