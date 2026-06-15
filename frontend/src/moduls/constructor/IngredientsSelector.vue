<template>
  .
  <div class="ingredients__filling">
    <p>Начинка:</p>
    <ul class="ingredients__list">
      <li
        v-for="ingredient in items"
        :key="ingredient.id"
        class="ingredients__item">
        <app-drag
          :data-transfer="ingredient"
          :draggable="getValue(ingredient.value) < MAX_INGREDIENT_COUNT">
          <div class="filling">
            <img
              :src="getImage(ingredient.image)"
              :alt="ingredient.name" />
            {{ ingredient.name }}
          </div>
        </app-drag>
        <div class="counter counter--orange ingredients__counter">
          <button
            type="button"
            class="counter__button counter__button--minus"
            :disabled="getValue(ingredientType.value) === 0"
            @click="decrementValue(ingredientType.value)">
            <span class="visually-hidden">Меньше</span>
          </button>
          <input
            type="text"
            name="counter"
            class="counter__input"
            :value="getValue(ingredient.value)"
            @input="inputValue(ingredient.value, $event.target.value)" />
          value="0" />
          <button
            type="button"
            class="counter__button counter__button--plus"
            :disabled="getValue(ingredient.value) === 0"
            @click="incrementValue(ingredient.value)">
            <span class="visually-hidden">Больше</span>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { toRef } from 'vue';
import AppDrag from '@/common/components/AppDrag.vue';
import { MAX_INGREDIENT_COUNT } from '@/common/constants';

const props = defineProps({
  values: {
    type: Object,
    default: () => ({}),
  },
  items: {
    type: Array,
    default: () => [],
  },
});
const emit = defineEmits(['update']);
const values = toRef(props, 'values');

const getValue = (ingredient) => {
  return values.value[ingredient] ?? 0;
};

const setValue = (ingredient, count) => {
  emit('update', ingredient, Number(count));
};

const decrementValue = (ingredient) => {
  setValue(ingredient, getValue(ingredient) - 1);
};

const incrementValue = (ingredient) => {
  setValue(ingredient, getValue(ingredient) + 1);
};

const inputValue = (ingredient, count) => {
  return setValue(ingredient, Math.min(MAX_INGREDIENT_COUNT, Number(count)));
};

const getImage = (image) => {
  return new URL(`../../assets/img/${image}`, import.meta.url).href;
};
</script>

<style lang="scss" scoped>
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

.filling {
  @include ds-typography.r-s14-h16;

  position: relative;

  display: block;

  padding-left: 36px;

  img {
    @include p_center-v;

    display: block;

    width: 32px;
    height: 32px;

    box-sizing: border-box;
    padding: 4px;

    border-radius: 50%;
  }

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

.counter {
  display: flex;

  justify-content: space-between;
  align-items: center;
}

.counter__button {
  $el: &;
  $size_icon: 50%;

  position: relative;

  display: block;

  width: 16px;
  height: 16px;
  margin: 0;
  padding: 0;

  cursor: pointer;
  transition: 0.3s;

  border: none;
  border-radius: 50%;
  outline: none;

  &--minus {
    background-color: ds-colors.$purple-100;

    &::before {
      @include m_center.p_center-all;

      width: $size_icon;
      height: 2px;

      content: '';

      border-radius: 2px;
      background-color: ds-colors.$black;
    }

    &:hover:not(:active):not(:disabled) {
      background-color: ds-colors.$purple-200;
    }

    &:active:not(:disabled) {
      background-color: ds-colors.$purple-300;
    }

    &:focus:not(:disabled) {
      box-shadow: ds-shadows.$shadow-regular;
    }

    &:disabled {
      cursor: default;

      &::before {
        opacity: 0.1;
      }
    }
  }

  &--plus {
    background-color: ds-colors.$green-500;

    &::before {
      @include m_center.p_center-all;

      width: $size_icon;
      height: 2px;

      content: '';

      border-radius: 2px;
      background-color: ds-colors.$white;
    }

    &::after {
      @include m_center.p_center-all;

      width: $size_icon;
      height: 2px;

      content: '';
      transform: translate(-50%, -50%) rotate(90deg);

      border-radius: 2px;
      background-color: ds-colors.$white;
    }

    &:hover:not(:active):not(:disabled) {
      background-color: ds-colors.$green-400;
    }

    &:active:not(:disabled) {
      background-color: ds-colors.$green-600;
    }

    &:focus:not(:disabled) {
      box-shadow: ds-shadows.$shadow-regular;
    }

    &:disabled {
      cursor: default;

      opacity: 0.3;
    }
  }

  &--orange {
    background-color: ds-colors.$orange-200;

    &:hover:not(:active):not(:disabled) {
      background-color: ds-colors.$orange-100;
    }

    &:active:not(:disabled) {
      background-color: ds-colors.$orange-300;
    }
  }
}

.counter__input {
  @include ds-typography.r-s14-h16;

  box-sizing: border-box;
  width: 22px;
  margin: 0;
  padding: 0 3px;

  text-align: center;

  color: ds-colors.$black;
  border: none;
  border-radius: 10px;
  outline: none;
  background-color: transparent;

  &:focus {
    box-shadow: inset ds-shadows.$shadow-regular;
  }
}
</style>
