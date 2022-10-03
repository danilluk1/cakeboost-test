<template>
  <div class="root" v-if="props.isEnabled">
    <div class="modal">
      <ExitCross class="exit-cross" @click="() => props.setEnabled(false)" />
      <div class="image" />
      <div class="prplel" />
      <div class="prpler" />
      <div class="left-block">
        <header>
          <div class="horizontal">
            <div>
              <span class="p-sale">10%</span>
              <span class="p-sale-off">off</span>
            </div>
          </div>
          <span class="p-forder">your first order</span>
          <div class="prple-rect"></div>
          <span class="p-promo"
            >Subscribe to recieve 10% off promocode plus exclusive offers and
            deals</span
          >
        </header>
        <main class="main">
          <div>
            <p class="email-text">Email-address</p>
            <input
              class="email-input"
              type="text"
              :value="email"
              @change="(event: any) => email = event.target.value"
            />
          </div>
          <button class="subscribe-btn" @click="onSubscribeClick">
            Subscribe!
          </button>
          <input
            type="checkbox"
            class="cstm-cb"
            id="checkb"
            :value="isAgree"
            @input="(event: any) => isAgree = event.target.checked"
          />
          <label class="cstm-cb-label" for="checkb"
            >I'm agree with privacy policy</label
          >

          <div class="message-success" v-if="!isContains && isShown">
            You have successfully subscribed to the newsletter
          </div>
          <div class="message-error" v-else-if="isContains && isShown">
            You have already subscribed to the newsletter
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onUnmounted, ref } from "vue";
import ExitCross from "../assets/exit_cross.svg";

const props = defineProps<{
  isEnabled: boolean;
  setEnabled: (val: boolean) => void;
}>();

const regex = new RegExp("[a-z0-9]+@[a-z]+\.[a-z]{2,3}");
const timer = ref(0);
const email = ref("");
const isContains = ref(false);
const isShown = ref(false);
const isAgree = ref(false);

onUnmounted(() => {
  clearTimeout(timer.value);
});

function onSubscribeClick() {
  if (!isAgree.value)
    return alert("Необходимо принять нашу политику конфидициальности.");

  if (!regex.test(email.value)) return alert("Введите валидный email");

  const registeredEmailsStr = localStorage.getItem("emails");
  if (!registeredEmailsStr) return alert("Нет доступа к локальному хранилищу");

  const registeredEmails: string[] = JSON.parse(registeredEmailsStr);
  //По хорошему надо отчистить эти данные
  if (!registeredEmails) return alert("В хранилище лежат неверные данные.");

  isContains.value = registeredEmails.includes(email.value);

  if (!isContains.value) {
    registeredEmails.push(email.value);
    localStorage.setItem("emails", JSON.stringify(registeredEmails));
  }

  isShown.value = true;
  timer.value = setTimeout(() => {
    isShown.value = false;
  }, 3000);
}
</script>
<style scoped>
.cstm-cb-label {
  font-family: "Roboto", sans-serif;
  font-size: 14px;
  line-height: 16px;
  font-weight: 500;
  color: #595e61;
}
.cstm-cb {
  box-sizing: border-box;
  position: absolute;
  z-index: -1;
  opacity: 0;
}

.cstm-cb + label {
  display: inline-flex;
  align-items: center;
  user-select: none;
}

.cstm-cb+label::before {
  content: '';
  display: inline-block;
  width: 16px;
  height: 16px;
  border: 2px solid #c24dfe;
  border-radius: 4px;
  margin-right: 7px;
  background-repeat: no-repeat;
  background-position: center;
  background-size: 8px;
}

.cstm-cb:checked + label::before {
  border-color: #c24dfe;
  background-color: white;
  background-repeat: no-repeat;
  background-position: center;
  background-image: url(../assets/cb.svg);
}

.cstm-cb:not(:disabled):not(:checked) + label:hover::before {
  border-color: #b3d7ff;
}


.message-success {
  margin-top: 5px;
  color: lightgreen;
  font-weight: 700;
  font-size: 12px;
  line-height: 100%;
}

.message-error {
  margin-top: 5px;
  color: red;
  font-weight: 700;
  font-size: 12px;
  line-height: 100%;
}
.root {
  font-family: 'Roboto', sans-serif;

  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 5;
}
.modal {
  position: absolute;
  display: flex;
  width: 864px;
  height: 502px;
  border-radius: 8px;
  box-shadow: 0px 8px 20px rgba(68, 75, 77, 0.15);
  background-color: white;
}
.left-block {
  display: flex;
  margin-left: 32px;
  margin-top: 40px;
  flex-direction: column;
  width: 40%;
}

.main {
  display: flex;
  flex-direction: column;
}

.p-sale {
  line-height: 100%;
  font-weight: 700;
  font-size: 104px;
  color: #2F3639;
}

.p-sale-off{
  font-weight: 700;
  font-size: 24px;
  color: #2F3639;
  line-height: 100%;
}

.p-forder {
  display: block;
  color: #828688;
  font-size: 24px;
  font-weight: 700;
  line-height: 100%;
  margin-bottom: 20px;
  margin-top: 15px;
}

.prple-rect{
  width: 67px;
  height: 2px;
  border-radius: 10px;
  background: #C24DFE;
  margin-bottom: 20px;
}

.p-promo {
  display: block;
  color: #595E61;
  font-size: 14px;
  font-weight: 400;
  margin-bottom: 40px;
}

.email-text {
  font-size: 12px;
  color: #828688;
  line-height: 14px;
  font-weight: 700;
  margin-bottom: 5px;
}

.email-input {
  border-radius: 6px;
  border: 1px solid #D5D7D7;
  width: 302px;
  height: 40px;
  outline: none;
  margin-bottom: 15px;
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  line-height: 20px;
  color: #2F3639;
  font-weight: 500;
  padding: 5px;
}


.subscribe-btn {
  width: 106px;
  height: 40px;
  border-radius: 35px;
  background-color: #C24DFE;
  border-width: 0;
  color: white;
  font-family: "Roboto", sans-serif;
  font-weight: bold;
  font-size: 12px;
  line-height: 14px;
  margin-bottom: 20px;
  cursor: pointer;
  transform: translateZ(0);
  transition: all 0.2s, box-shadow 0.08s ease-in;
  box-shadow: rgba(50, 50, 93, 0.1) 0 0 0 1px inset, rgba(50, 50, 93, 0.1) 0 2px 5px 0,
    rgba(0, 0, 0, 0.07) 0 1px 1px 0;
}

.subscribe-btn:hover {
  box-shadow: rgba(50, 50, 93, 0.1) 0 0 0 1px inset, rgba(50, 50, 93, 0.2) 0 6px 15px 0,
    rgba(0, 0, 0, 0.1) 0 2px 2px 0, rgba(50, 151, 211, 0.3) 0 0 0 4px;
}
.exit-cross {
  cursor: pointer;
  position: absolute;
  right: 12px;
  top: 12px;
  z-index: 6;
}

.image {
  background: url("../assets/heroes.png") no-repeat;
  position: absolute;
  width: 582px;
  height: 502px;
  left: 283px;
  z-index: 5;
  border-bottom-right-radius: 6px;
}

.prplel {
  background: url("../assets/prpler.png") no-repeat;
  position: absolute;
  width: 154px;
  height: 200px;
  top: 0px;
  left: 748px;
  transform: rotate(-180deg);
}

.prpler {
  background: url("../assets/prpler.png") no-repeat;
  position: absolute;
  width: 224px;
  height: 300px;
  left: 355px;
  top: 202px;
}

</style>
