<template>
  <main class="content">
    <form
      action="#"
      method="post">
      <div class="content__wrapper">
        <h1 class="title title--big">Конструктор пиццы</h1>

        <dough-selector
          v-model="pizza.dough"
          :items="doughItems" />
        <diameter-selector
          v-model="pizza.size"
          :items="sizeItems" />

        <div class="content__ingredients">
          <div class="sheet">
            <h2 class="title title--small sheet__title">Выберите ингредиенты</h2>

            <div class="sheet__content ingredients"></div>

            <div class="ingredients__filling">
              <p>Начинка:</p>
              <sauce-selector
                v-model="pizza.sauce"
                :items="sauceItems" />
              <ingredients-selector
                :values="pizza.ingredients"
                :items="ingredientItems"
                @update="updateIngredientAmount" />
            </div>
          </div>

          <pizza-constructor
            :dough="pizza.dough"
            :sauce="pizza.sauce"
            :ingredients="pizza.ingredients"
            @drop="addIngredient" />
        </div>
      </div>

      <div class="content__pizza">
        <label class="input">
          <span class="visually-hidden">Название пиццы</span>
          <input
            type="text"
            name="pizza_name"
            placeholder="Введите название пиццы" />
        </label>

        <div class="content__result">
          <p>Итого: 0 ₽</p>
          <button
            type="button"
            class="button"
            disabled>
            Готовьте!
          </button>
        </div>
      </div>
    </form>
  </main>
</template>

<script setup>
import DoughSelector from '@/moduls/constructor/DoughSelector.vue';
import DiameterSelector from '@/modules/constructor/DiameterSelector.vue';
import SauceSelector from '@/modules/constructor/SauceSelector.vue';
import IngredientsSelector from '@/modules/constructor/IngredientsSelector.vue';
import PizzaConstructor from '@/modules/constructor/PizzaConstructor.vue';
import {
  normalizeDough,
  normalizeIngredients,
  normalizeSauces,
  normalizeSize,
} from '@/common/helpers/normalize';

import doughJSON from '@/mocks/dough.json';
import ingredientsJSON from '@/mocks/ingredients.json';
import saucesJSON from '@/mocks/sauces.json';
import sizesJSON from '@/mocks/sizes.json';

const doughItems = doughJSON.map(normalizeDough);
const ingredientItems = ingredientsJSON.map(normalizeIngredients);
const sauceItems = saucesJSON.map(normalizeSauces);
const sizeItems = sizesJSON.map(normalizeSize);

// const images = import.meta.glob('../assets/img/**/*', {
//   eager: true,
//   import: 'default',
//   query: '?url',
// });

// const getImage = (image) => {
//   return images[`../assets/img/${image}`];
// };

const pizza = reactive({
  name: '',
  dough: doughItems[0].value,
  size: sizeItems[0].value,
  sauce: sauceItems[0].value,
  ingredients: ingredientItems.reduce((acc, item) => {
    acc[item.value] = 0;
    return acc;
  }, {}),
});

const price = computed(() => {
  const { dough, size, sauce, ingredients } = pizza;

  const sizeMultiplier = sizeItems.find((item) => item.value === size)?.multiplier ?? 1;

  const doughPrice = doughItems.find((item) => item.value === dough)?.price ?? 0;

  const saucePrice = sauceItems.find((item) => item.value === sauce)?.price ?? 0;

  /*
   * Здесь мы при помощи метода map превращаем массив ингредиентов
   * в массив значений, соответствующих итоговой стоимости каждого из них — просто умножаем цену на количество.
   * После чего методом reduce вычисляем сумму всех элементов массива. Это даёт нам общую стоимость ингредиентов.
   */
  const ingredientsPrice = ingredientItems
    .map((item) => ingredients[item.value] * item.price)
    .reduce((acc, item) => acc + item, 0);

  return (doughPrice + saucePrice + ingredientsPrice) * sizeMultiplier;
});

const disableSubmit = computed(() => {
  return pizza.name.length === 0 || price.value === 0;
});
const addIngredient = (ingredient) => {
  pizza.ingredients[ingredient]++;
};
const updateIngredientAmount = (ingredient, count) => {
  pizza.ingredients[ingredient] = count;
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/ds-system/ds-typography';
@use '@/assets/scss/ds-system/ds-colors';
@use '@/assets/scss/ds-system/ds-shadows';
@use '@/assets/scss/mixins/m_center';
@use '@/assets/scss/mixins/m_clear-list';
@import '@/assets/scss/ds-system/ds.scss';
@import '@/assets/scss/mixins/mixins.scss';
.content {
  padding-top: 20px;
}

.content__wrapper {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;

  width: 920px;
  margin: 0 auto;
  padding-right: 2.12%;
  padding-bottom: 30px;
  padding-left: 2.12%;
}

.content__ingredients {
  width: 527px;
  margin-top: 15px;
  margin-right: auto;
  margin-bottom: 15px;
}

.content__pizza {
  width: 373px;
  margin-top: 15px;
  margin-bottom: 15px;
}

.content__result {
  display: flex;
  align-items: center;
  justify-content: center;

  margin-top: 25px;

  p {
    @include ds-typography.b-s24-h28;

    margin: 0;
  }

  button {
    margin-left: 12px;
    padding: 16px 45px;
  }
}

.sheet {
  padding-top: 15px;

  border-radius: 8px;
  background-color: ds-colors.$white;
  box-shadow: ds-shadows.$shadow-light;
}

.sheet__title {
  padding-right: 18px;
  padding-left: 18px;
}

.sheet__content {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  margin-top: 8px;
  padding-top: 18px;
  padding-right: 18px;
  padding-left: 18px;

  border-top: 1px solid rgba(ds-colors.$green-500, 0.1);
}

.title {
  box-sizing: border-box;
  width: 100%;
  margin: 0;

  color: ds-colors.$black;

  &--big {
    @include ds-typography.b-s36-h42;
  }

  &--small {
    @include ds-typography.b-s18-h21;
  }
}

.radio {
  cursor: pointer;

  span {
    @include ds-typography.r-s16-h19;

    position: relative;

    padding-left: 28px;

    &:before {
      @include m_center.p_center-v;

      display: block;

      box-sizing: border-box;
      width: 20px;
      height: 20px;

      content: '';
      transition: 0.3s;

      border: 1px solid ds-colors.$purple-400;
      border-radius: 50%;
      background-color: ds-colors.$white;
    }
  }

  &:hover {
    input:not(:checked):not(:disabled) + span {
      &:before {
        border-color: ds-colors.$purple-800;
      }
    }
  }

  input {
    display: none;

    &:checked + span {
      &:before {
        border: 6px solid ds-colors.$green-500;
      }
    }

    &:disabled {
      & + span {
        &:before {
          border-color: ds-colors.$purple-400;
          background-color: ds-colors.$silver-200;
        }
      }

      &:checked + span {
        &:before {
          border: 6px solid ds-colors.$purple-400;
        }
      }
    }
  }
}

.button {
  $bl: &;

  @include ds-typography.b-s18-h21;
  font-family: inherit;
  display: block;

  box-sizing: border-box;
  margin: 0;
  padding: 0;

  cursor: pointer;
  transition: 0.3s;
  text-align: center;

  color: ds-colors.$white;
  border: none;
  border-radius: 8px;
  outline: none;
  box-shadow: ds-shadows.$shadow-medium;

  background-color: ds-colors.$green-500;

  &:hover:not(:active):not(:disabled) {
    background-color: ds-colors.$green-400;
  }

  &:active:not(:disabled) {
    background-color: ds-colors.$green-600;
  }

  &:focus:not(:disabled) {
    opacity: 0.5;
  }

  &:disabled {
    background-color: ds-colors.$green-300;
    color: rgba(ds-colors.$white, 0.2);
    cursor: default;
  }

  &--border {
    background-color: transparent;
    border: 1px solid ds-colors.$green-500;
    color: ds-colors.$black;
    box-shadow: none;

    &:hover:not(:active):not(:disabled) {
      color: ds-colors.$green-500;
      border-color: ds-colors.$green-500;
      background-color: transparent;
    }

    &:active:not(:disabled) {
      color: ds-colors.$green-600;
      border-color: ds-colors.$green-600;
      background-color: transparent;
    }

    &:disabled {
      opacity: 0.5;
    }
  }

  &--transparent {
    @include ds-typography.b-s14-h16;
    background-color: transparent;
    box-shadow: none;
    color: ds-colors.$black;

    &:hover:not(:active):not(:disabled) {
      color: ds-colors.$red-800;
      background-color: transparent;
    }

    &:active:not(:disabled) {
      color: ds-colors.$red-900;
      background-color: transparent;
    }

    &:disabled {
      opacity: 0.25;
    }
  }

  &--arrow {
    &::before {
      content: '';
      background-image: url('../img/button-arrow.svg');
      background-position: center;
      background-repeat: no-repeat;
      margin-right: 16px;
      width: 18px;
      height: 18px;
      display: inline-block;
      vertical-align: middle;
      transform: translateY(-1px);
    }
  }

  &--white {
    background-color: ds-colors.$white;
    color: ds-colors.$green-500;
  }
}

.pizza {
  position: relative;

  display: block;

  box-sizing: border-box;
  width: 100%;

  background-repeat: no-repeat;
  background-position: center;
  background-size: 100%;

  &--foundation--big-creamy {
    background-image: url('../img/foundation/big-creamy.svg');
  }

  &--foundation--big-tomato {
    background-image: url('../img/foundation/big-tomato.svg');
  }

  &--foundation--small-creamy {
    background-image: url('../img/foundation/small-creamy.svg');
  }

  &--foundation--small-tomato {
    background-image: url('../img/foundation/small-tomato.svg');
  }
}

.input {
  display: block;

  span {
    @include ds-typography.r-s14-h16;

    display: block;

    margin-bottom: 4px;
  }

  input {
    @include ds-typography.r-s16-h19;

    display: block;

    box-sizing: border-box;
    width: 100%;
    margin: 0;
    padding: 8px 16px;

    transition: 0.3s;

    color: ds-colors.$black;
    border: 1px solid ds-colors.$purple-400;
    border-radius: 8px;
    outline: none;
    background-color: ds-colors.$white;

    font-family: inherit;

    &:focus {
      border-color: ds-colors.$green-500;
    }
  }

  &:hover {
    input {
      border-color: ds-colors.$black;
    }
  }

  &--big-label {
    display: flex;
    align-items: center;

    span {
      @include ds-typography.b-s16-h19;

      margin-right: 16px;

      white-space: nowrap;
    }
  }
}
</style>
