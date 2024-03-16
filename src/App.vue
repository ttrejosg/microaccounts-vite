<template>
  <main>
    <h1>Collection microaccounts</h1>
    <div class="dishes-container">
      <section>
        <h2 class="orange">ADD DISH</h2>
        <form @submit.prevent="submit">
          <div class="input-box">
            <input
              type="text"
              name="name"
              v-model="name"
              required
              autocomplete="off"
              pattern="[^0-9]*|[a-zA-Z]*"
              title="The name of the dish cannot contains any digits"
            />
            <label for="name">Enter a dish name</label>
          </div>
          <div class="input-box">
            <input
              type="text"
              name="price"
              v-model.number="price"
              required
              autocomplete="off"
              pattern="[0-9]*.[0-9]+"
              title="The price of the dish can only contain digits"
            />
            <label for="price">Enter the price</label>
          </div>
          <div class="input-box">
            <input
              type="text"
              name="amount"
              v-model.number="amount"
              required
              autocomplete="off"
              pattern="[0-9]*|[^a-zA-Z]*"
              title="The amount of the dish can only contain digits and be a decimal number"
            />
            <label for="amount">Enter the amount</label>
          </div>

          <input type="submit" value="Add Dish" />
        </form>
      </section>
      <section v-show="dishes.length > 0">
        <h2 class="blue">DISHES LIST</h2>
        <ul>
          <li v-for="(item, index) in dishes" :key="index">
            <div class="dish-info">
              <p>{{ index + 1 }}.</p>
              <h5>{{ item.name }}</h5>
              <span>${{ item.price }}</span>
            </div>
            <div class="dish-money">
              <div>
                <h4>Amount:</h4>
                <span>{{ item.amount }} units</span>
              </div>
              <div>
                <h4>Total:</h4>
                <span>$ {{ item.price * item.amount }}</span>
              </div>
            </div>
            <button @click="deleteDish" :data-index="index">Delete Dish</button>
          </li>
        </ul>
      </section>
    </div>
    <section v-show="dishes.length > 0">
      <h2 class="green">TOTAL COUNT</h2>
      <p class="total">
        {{ total }}
      </p>
    </section>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
const dishes = ref([]);
const name = ref("");
const price = ref("");
const amount = ref("");
const total = computed(() => {
  const totalTemp = dishes.value.reduce((acc, item) => {
    return acc + item.price * item.amount;
  }, 0);
  return totalTemp === 0 ? "No hay un pago pendiente" : totalTemp;
});

const submit = (e) => {
  dishes.value.push({
    name: name.value,
    price: price.value,
    amount: amount.value,
  });

  name.value = "";
  price.value = "";
  amount.value = "";
};

const deleteDish = (e) => {
  dishes.value = dishes.value.filter(
    (item, index) => index != e.target.dataset.index
  );
};
</script>

<style scoped>
.orange {
  color: #e97d02;
}

.blue {
  color: #0573b8;
}

.green {
  color: #1da911;
}

h1 {
  text-align: center;
  margin: 0;
  padding: 0;
  font-size: 3.5rem;
  text-transform: uppercase;
  color: #e97d02;
}

h2 {
  font-size: 2.5rem;
  font-weight: 600;
  margin: 0;
  margin-bottom: 1rem;
  text-align: center;
}

main {
  width: 100%;
}

.dishes-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  margin-top: 2rem;
  padding: 0 20%;
}

section {
  display: flex;
  flex-direction: column;
  align-items: center;
}

form {
  display: flex;
  gap: 2rem;
  width: 100%;
  margin-top: 1rem;
}

.input-box {
  position: relative;
  width: 96%;
}

.input-box input {
  width: 100%;
  padding: 10px;
  border: 1.5px solid #e97d02;
  border-radius: 5px;
  outline: none;
  font-size: 1rem;
  transition: 0.8s;
  background: rgb(12, 16, 32);
  color: white;
}

.input-box label {
  position: absolute;
  left: 0;
  top: 0;
  transform: translateX(10px) translateY(-11px);
  padding: 10px;
  pointer-events: none;
  text-transform: uppercase;
  font-size: 0.8rem;
  padding: 1.5px 10px;
  background: #e97d02;
  color: white;
  border-radius: 2px;
  letter-spacing: 0.5px;
  transition: 0.5s;
}

.input-box input:focus {
  border: 1.5px solid #0573b8;
}

.input-box input:valid {
  border: 1.5px solid #1da911;
}

.input-box input:focus ~ label {
  background: #0573b8;
  color: white;
}

.input-box input:valid ~ label {
  background: #1da911;
}

input[type="submit"] {
  padding: 10px;
  border: none;
  background: #e6912f;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.5s;
}

input[type="submit"]:hover,
input[type="submit"]:focus {
  scale: 1.05;
  background: #e97d02;
  outline: none;
}
ul {
  display: flex;
  gap: 2rem;
}

li {
  list-style: none;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
  background: #1e2747;
  border-radius: 5px;
}

span {
  font-size: 1.3rem;
  font-weight: 400;
  color: white;
  background: #0573b8;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  margin: 0 0.5rem;
}

.dish-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  p {
    font-size: 1.4rem;
    font-weight: 600;
    margin: 0;
    margin-right: 0.1rem;
  }
  h5 {
    font-size: 1.4rem;
    font-weight: 600;
    margin: 0;
  }
}

.dish-money {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  div {
    display: flex;
    gap: 0.5rem;
    justify-content: space-between;
  }
  h4 {
    margin: 0;
    font-size: 1.4rem;
    font-weight: 600;
  }
}

button {
  padding: 10px;
  border: none;
  background: #478fbc;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.5s;
}

button:hover {
  scale: 1.05;
  outline: none;
  background: #0573b8;
}

.total {
  font-size: 2rem;
  font-weight: 600;
  background: #1da911;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 5px;
  text-align: center;
  margin: 0;
}
</style>
