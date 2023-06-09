<template>
  <div class="px-10 py-4">
    <label for="search" class="relative flex justify-center items-center">
      <input
        id="search"
        type="search"
        placeholder="Search products"
        v-model="query"
        class="border w-full py-4 my-4 rounded-md focus:outline-none focus:border-transparent focus:ring focus:ring-offset-2 focus:ring-orange-100 focus:ring-offset-orange-800"
      />
      <MagnifyingGlassIcon class="absolute top-1/3.5 right-4 h-5 w-5 text-gray-400" />
    </label>

    <h2 class="text-2xl font-semibold">Products</h2>
    <div class="border-b my-2"></div>
    <!-- Card -->
    <TransitionGroup
      name="list"
      tag="div"
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 mt-4"
    >
      <div
        role="button"
        tabindex="-1"
        v-for="item in filteredItems"
        :key="item.id"
        class="bg-white rounded-md group shadow-md overflow-hidden cursor-pointer hover:shadow-lg active:shadow-inner active:bg-gray-100 active:scale-95 transition-all disabled:opacity-50 disabled:cursor-not-allowed"
        @click="router.push(`/products/${item.id}`)"
      >
        <div class="relative">
          <NuxtImg
            :src="item.image"
            :alt="item.name"
            class="w-full h-48 object-cover object-center group-hover:opacity-75 transition-opacity"
            loading="lazy"
          />
          <div class="absolute top-2 right-2">
            <button
              class="snipcart-add-item bg-white rounded-full p-2 focus:outline-none focus:ring focus:ring-offset-2 focus:ring-orange-100 focus:ring-offset-orange-800"
              :data-item-id="item.id"
              :data-item-price="item.price"
              :data-item-description="item.description"
              :data-item-image="item.image"
              :data-item-name="item.name"
              :data-item-url="`/products/${item.id}`"
              :data-item-custom1-name="`Logged In User Email`"
              :data-item-custom1-type="'hidden'"
              :data-item-custom1-value="user?.email"
            >
              <ShoppingBagIcon class="h-6 w-6 text-primary-200" />
            </button>
          </div>
        </div>
        <div class="p-4">
          <h3 class="text-lg font-semibold">{{ item.name }}</h3>
          <p class="text-gray-500">${{ item.price?.toFixed(2) }}</p>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script setup lang="ts">
definePageMeta({ middleware: "auth" });
import { MagnifyingGlassIcon, ShoppingBagIcon } from "@heroicons/vue/24/outline";

const { getSession } = useAuth();
const router = useRouter();

const { user } = await getSession();

const query = ref("");
const items = useProducts();

const filteredItems = computed(() => {
  return items.value.filter((item) => item.name.toLowerCase().includes(query.value));
});
</script>

<style scoped lang="scss">
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease-out;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
