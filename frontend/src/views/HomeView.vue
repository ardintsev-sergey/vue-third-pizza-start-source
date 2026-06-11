<template>
  <main class="content">
    <form
      action="#"
      method="post">
      <div class="content__wrapper">
        <h1 class="title title--big">Конструктор пиццы</h1>

        <div class="content__dough">
          <div class="sheet">
            <h2 class="title title--small sheet__title">Выберите тесто</h2>

            <div class="sheet__content dough">
              <label
                v-for="dough in doughItems"
                :key="dough.id"
                class="dough__input">
                <input
                  type="radio"
                  name="dought"
                  :value="dough.value"
                  class="visually-hidden"
                  checked />
                <img
                  :src="getImage(dough.image)"
                  :alt="dough.name" />

                <b>{{ dough.name }}</b>
                <span>{{ dough.descriptiom }}</span>
              </label>
            </div>
          </div>
        </div>

        <div class="content__diameter">
          <div class="sheet">
            <h2 class="title title--small sheet__title">Выберите размер</h2>

            <div class="sheet__content diameter">
              <label
                v-for="size in sizeItems"
                :key="size.id"
                class="diameter__input"
                :class="`diameter__input--${size.value}`">
                <input
                  type="radio"
                  name="diameter"
                  :value="size.value"
                  class="visually-hidden" />
                <span>{{ size.name }}</span>
              </label>
            </div>
          </div>
        </div>

        <div class="content__ingredients">
          <div class="sheet">
            <h2 class="title title--small sheet__title">Выберите ингредиенты</h2>

            <div class="sheet__content ingredients">
              <div class="ingredients__sauce">
                <p>Основной соус:</p>

                <label
                  v-for="sauce in sauceItems"
                  :key="sauce.id"
                  class="radio ingredients__input">
                  <input
                    type="radio"
                    :value="sauce.value" />
                  <span>{{ sauce.name }}</span>
                </label>
              </div>

              <div class="ingredients__filling">
                <p>Начинка:</p>

                <ul class="ingredients__list">
                  <li
                    v-for="ingredient in ingredientItems"
                    :key="ingredient.id"
                    class="ingredients__item">
                    <div class="filling">
                      <img
                        :src="getImage(ingredient.image)"
                        :alt="ingredient.name" />
                      {{ ingredient.name }}
                    </div>

                    <div class="counter counter--orange ingredients__counter">
                      <button
                        type="button"
                        class="counter__button counter__button--minus"
                        disabled>
                        <span class="visually-hidden">Меньше</span>
                      </button>
                      <input
                        type="text"
                        name="counter"
                        class="counter__input"
                        value="0" />
                      <button
                        type="button"
                        class="counter__button counter__button--plus">
                        <span class="visually-hidden">Больше</span>
                      </button>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
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

          <div class="content__constructor">
            <div class="pizza pizza--foundation--big-tomato">
              <div class="pizza__wrapper">
                <div class="pizza__filling pizza__filling--ananas"></div>
                <div class="pizza__filling pizza__filling--bacon"></div>
                <div class="pizza__filling pizza__filling--cheddar"></div>
              </div>
            </div>
          </div>

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
      </div>
    </form>
  </main>
</template>

<script setup>
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

const images = import.meta.glob('../assets/img/**/*', {
  eager: true,
  import: 'default',
  query: '?url',
});

const getImage = (image) => {
  return images[`../assets/img/${image}`];
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/ds-system/ds-typography';
@use '@/assets/scss/ds-system/ds-colors';
@use '@/assets/scss/ds-system/ds-shadows';
@use '@/assets/scss/mixins/m_center';
@use '@/assets/scss/mixins/m_clear-list';
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

.content__dough {
  width: 527px;
  margin-top: 15px;
  margin-right: auto;
  margin-bottom: 15px;
}

.content__diameter {
  width: 373px;
  margin-top: 15px;
  margin-bottom: 15px;
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

.content__constructor {
  width: 315px;
  margin-top: 25px;
  margin-right: auto;
  margin-left: auto;
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

.dough__input {
  position: relative;

  margin-right: 8%;
  margin-bottom: 20px;
  padding-left: 50px;

  cursor: pointer;

  b {
    @include ds-typography.r-s16-h19;

    &::before {
      @include m_center.p_center-v;

      width: 36px;
      height: 36px;

      content: '';
      transition: 0.3s;

      border-radius: 50%;
      background-repeat: no-repeat;
      background-position: center;
      background-size: cover;
    }
  }

  span {
    @include ds-typography.l-s11-h13;

    display: block;
  }

  &--light {
    b {
      &::before {
        background-image: url('../img/dough-light.svg');
      }
    }
  }

  &--large {
    b {
      &::before {
        background-image: url('../img/dough-large.svg');
      }
    }
  }

  &:hover {
    b::before {
      box-shadow: ds-shadows.$shadow-regular;
    }
  }

  input {
    &:checked + b::before {
      box-shadow: ds-shadows.$shadow-large;
    }
  }
}

.diameter__input {
  margin-right: 8.7%;
  margin-bottom: 20px;
  padding-top: 7px;
  padding-bottom: 6px;

  cursor: pointer;

  span {
    @include ds-typography.r-s16-h19;

    position: relative;

    padding-left: 46px;

    &::before {
      @include m_center.p_center_v;

      width: 36px;
      height: 36px;

      content: '';
      transition: 0.3s;

      border-radius: 50%;
      background-color: ds-colors.$green-100;
      background-image: url('../img/diameter.svg');
      background-repeat: no-repeat;
      background-position: center;
    }
  }

  &:nth-child(3n) {
    margin-right: 0;
  }

  &--small {
    span::before {
      background-size: 18px;
    }
  }

  &--normal {
    span::before {
      background-size: 29px;
    }
  }

  &--big {
    span::before {
      background-size: 100%;
    }
  }

  &:hover {
    span::before {
      box-shadow: ds-shadows.$shadow-regular;
    }
  }

  input {
    &:checked + span::before {
      box-shadow: ds-shadows.$shadow-large;
    }
  }
}
.filling {
  @include ds-typography.r-s14-h16;

  position: relative;

  display: block;

  padding-left: 36px;

  &::before {
    @include m_center.p_center-v;

    display: block;

    width: 32px;
    height: 32px;

    content: '';

    border-radius: 50%;
    background-color: ds-colors.$white;
    background-repeat: no-repeat;
    background-position: center;
    background-size: 80% 80%;
  }

  &--tomatoes::before {
    background-image: url('../img/filling/tomatoes.svg');
  }

  &--ananas::before {
    background-image: url('../img/filling/ananas.svg');
  }

  &--bacon::before {
    background-image: url('../img/filling/bacon.svg');
  }

  &--blue_cheese::before {
    background-image: url('../img/filling/blue_cheese.svg');
  }

  &--cheddar::before {
    background-image: url('../img/filling/cheddar.svg');
  }

  &--chile::before {
    background-image: url('../img/filling/chile.svg');
  }

  &--ham::before {
    background-image: url('../img/filling/ham.svg');
  }

  &--jalapeno::before {
    background-image: url('../img/filling/jalapeno.svg');
  }

  &--mozzarella::before {
    background-image: url('../img/filling/mozzarella.svg');
  }

  &--mushrooms::before {
    background-image: url('../img/filling/mushrooms.svg');
  }

  &--olives::before {
    background-image: url('../img/filling/olives.svg');
  }

  &--onion::before {
    background-image: url('../img/filling/onion.svg');
  }

  &--parmesan::before {
    background-image: url('../img/filling/parmesan.svg');
  }

  &--salami::before {
    background-image: url('../img/filling/salami.svg');
  }

  &--salmon::before {
    background-image: url('../img/filling/salmon.svg');
  }
}

.ingredients__sauce {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  width: 100%;
  margin-bottom: 14px;

  p {
    @include ds-typography.r-s16-h19;

    margin-top: 0;
    margin-right: 16px;
    margin-bottom: 10px;
  }
}

.ingredients__input {
  margin-right: 24px;
  margin-bottom: 10px;
}

.ingredients__filling {
  width: 100%;

  p {
    @include ds-typography.r-s16-h19;

    margin-top: 0;
    margin-bottom: 16px;
  }
}

.ingredients__list {
  @include m_clear-list.clear-list;

  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
}

.ingredients__item {
  width: 100px;
  min-height: 40px;
  margin-right: 17px;
  margin-bottom: 35px;
}

.ingredients__counter {
  width: 54px;
  margin-top: 10px;
  margin-left: 36px;
}
</style>
