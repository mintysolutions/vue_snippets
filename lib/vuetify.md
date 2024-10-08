# Vue Code Examples

## Vuetify Example

### Tabs

```vue
<template>
  <v-tabs v-model="activeTab" grow>
    <v-tab :value="1">Choose Slot</v-tab>
    <v-tab :value="2">Contacts</v-tab>
    <v-tab :value="3">Confirmation</v-tab>
  </v-tabs>
  <v-tabs-window v-model="activeTab">
    <v-tabs-window-item :value="1" :key="1">
      <!-- Content for slot 1 -->
    </v-tabs-window-item>
    <v-tabs-window-item :value="2" :key="2">
      <!-- Content for slot 2 -->
    </v-tabs-window-item>
    <v-tabs-window-item :value="3" :key="3">
      <!-- Content for slot 3 -->
    </v-tabs-window-item>
  </v-tabs-window>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 1
    };
  }
};
</script>
```

### Data-Table

```vue
<template>
  <v-data-table-server
    v-model:items-per-page="itemsPerPage"
    :headers="headers"
    :items="serverItems"
    :items-length="totalItems"
    :loading="loading"
    item-value="name"
    @update:options="loadItems"
  ></v-data-table-server>
</template>

<script setup>
import { ref } from 'vue';

const cars = [
  {
    name: 'Ford Mustang',
    horsepower: 450,
    fuel: 'Gasoline',
    origin: 'USA',
    price: 55000
  },
  {
    name: 'Tesla Model S',
    horsepower: 670,
    fuel: 'Electric',
    origin: 'USA',
    price: 79999
  },
  {
    name: 'BMW M3',
    horsepower: 503,
    fuel: 'Gasoline',
    origin: 'Germany',
    price: 70000
  },
  {
    name: 'Audi RS6',
    horsepower: 591,
    fuel: 'Gasoline',
    origin: 'Germany',
    price: 109000
  },
  {
    name: 'Chevrolet Camaro',
    horsepower: 650,
    fuel: 'Gasoline',
    origin: 'USA',
    price: 62000
  },
  {
    name: 'Porsche 911',
    horsepower: 379,
    fuel: 'Gasoline',
    origin: 'Germany',
    price: 101000
  },
  {
    name: 'Jaguar F-Type',
    horsepower: 575,
    fuel: 'Gasoline',
    origin: 'UK',
    price: 61000
  },
  {
    name: 'Mazda MX-5',
    horsepower: 181,
    fuel: 'Gasoline',
    origin: 'Japan',
    price: 26000
  },
  {
    name: 'Nissan GT-R',
    horsepower: 565,
    fuel: 'Gasoline',
    origin: 'Japan',
    price: 113540
  },
  {
    name: 'Mercedes-AMG GT',
    horsepower: 523,
    fuel: 'Gasoline',
    origin: 'Germany',
    price: 115900
  }
];

const FakeAPI = {
  async fetch({ page, itemsPerPage, sortBy }) {
    return new Promise((resolve) => {
      setTimeout(() => {
        const start = (page - 1) * itemsPerPage;
        const end = start + itemsPerPage;
        const items = cars.slice();
        if (sortBy.length) {
          const sortKey = sortBy[0].key;
          const sortOrder = sortBy[0].order;
          items.sort((a, b) => {
            const aValue = a[sortKey];
            const bValue = b[sortKey];
            return sortOrder === 'desc' ? bValue - aValue : aValue - bValue;
          });
        }
        const paginated = items.slice(start, end);
        resolve({ items: paginated, total: items.length });
      }, 500);
    });
  }
};
const itemsPerPage = ref(5);
const headers = ref([
  { title: 'Car Model', key: 'name', align: 'start' },
  { title: 'Horsepower', key: 'horsepower', align: 'end' },
  { title: 'Fuel Type', key: 'fuel', align: 'start' },
  { title: 'Origin', key: 'origin', align: 'start' },
  { title: 'Price ($)', key: 'price', align: 'end' }
]);
const serverItems = ref([]);
const loading = ref(true);
const totalItems = ref(0);
function loadItems({ page, itemsPerPage, sortBy }) {
  loading.value = true;
  FakeAPI.fetch({ page, itemsPerPage, sortBy }).then(({ items, total }) => {
    serverItems.value = items;
    totalItems.value = total;
    loading.value = false;
  });
}
</script>

<script>
const desserts = [
  {
    name: 'Frozen Yogurt',
    calories: 159,
    fat: 6.0,
    carbs: 24,
    protein: 4.0,
    iron: '1'
  },
  {
    name: 'Jelly bean',
    calories: 375,
    fat: 0.0,
    carbs: 94,
    protein: 0.0,
    iron: '0'
  },
  {
    name: 'KitKat',
    calories: 518,
    fat: 26.0,
    carbs: 65,
    protein: 7,
    iron: '6'
  },
  {
    name: 'Eclair',
    calories: 262,
    fat: 16.0,
    carbs: 23,
    protein: 6.0,
    iron: '7'
  },
  {
    name: 'Gingerbread',
    calories: 356,
    fat: 16.0,
    carbs: 49,
    protein: 3.9,
    iron: '16'
  },
  {
    name: 'Ice cream sandwich',
    calories: 237,
    fat: 9.0,
    carbs: 37,
    protein: 4.3,
    iron: '1'
  },
  {
    name: 'Lollipop',
    calories: 392,
    fat: 0.2,
    carbs: 98,
    protein: 0,
    iron: '2'
  },
  {
    name: 'Cupcake',
    calories: 305,
    fat: 3.7,
    carbs: 67,
    protein: 4.3,
    iron: '8'
  },
  {
    name: 'Honeycomb',
    calories: 408,
    fat: 3.2,
    carbs: 87,
    protein: 6.5,
    iron: '45'
  },
  {
    name: 'Donut',
    calories: 452,
    fat: 25.0,
    carbs: 51,
    protein: 4.9,
    iron: '22'
  }
];

const FakeAPI = {
  async fetch({ page, itemsPerPage, sortBy }) {
    return new Promise((resolve) => {
      setTimeout(() => {
        const start = (page - 1) * itemsPerPage;
        const end = start + itemsPerPage;
        const items = desserts.slice();

        if (sortBy.length) {
          const sortKey = sortBy[0].key;
          const sortOrder = sortBy[0].order;
          items.sort((a, b) => {
            const aValue = a[sortKey];
            const bValue = b[sortKey];
            return sortOrder === 'desc' ? bValue - aValue : aValue - bValue;
          });
        }

        const paginated = items.slice(start, end);

        resolve({ items: paginated, total: items.length });
      }, 500);
    });
  }
};

export default {
  data: () => ({
    itemsPerPage: 5,
    headers: [
      {
        title: 'Dessert (100g serving)',
        align: 'start',
        sortable: false,
        key: 'name'
      },
      { title: 'Calories', key: 'calories', align: 'end' },
      { title: 'Fat (g)', key: 'fat', align: 'end' },
      { title: 'Carbs (g)', key: 'carbs', align: 'end' },
      { title: 'Protein (g)', key: 'protein', align: 'end' },
      { title: 'Iron (%)', key: 'iron', align: 'end' }
    ],
    serverItems: [],
    loading: true,
    totalItems: 0
  }),
  methods: {
    loadItems({ page, itemsPerPage, sortBy }) {
      this.loading = true;
      FakeAPI.fetch({ page, itemsPerPage, sortBy }).then(({ items, total }) => {
        this.serverItems = items;
        this.totalItems = total;
        this.loading = false;
      });
    }
  }
};
</script>
```

### Table filter

```vue
<template>
  <v-card flat>
    <v-card-title class="d-flex align-center pe-2">
      <v-icon icon="mdi-video-input-component"></v-icon>&nbsp; Find a Graphics Card
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        density="compact"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="solo-filled"
        flat
        hide-details
        single-line
      ></v-text-field>
    </v-card-title>

    <v-divider></v-divider>
    <v-data-table :items="items" v-model:search="search">
      <template v-slot:header.stock>
        <div class="text-end">Stock</div>
      </template>

      <template v-slot:item.image="{ item }">
        <v-card class="my-2" elevation="2" rounded>
          <v-img
            :src="`https://cdn.vuetifyjs.com/docs/images/graphics/gpus/${item.image}`"
            height="64"
            cover
          ></v-img>
        </v-card>
      </template>

      <template v-slot:item.rating="{ item }">
        <v-rating
          :model-value="item.rating"
          color="orange-darken-2"
          density="compact"
          size="small"
          readonly
        ></v-rating>
      </template>

      <template v-slot:item.stock="{ item }">
        <div class="text-end">
          <v-chip
            :color="item.stock ? 'green' : 'red'"
            :text="item.stock ? 'In stock' : 'Out of stock'"
            class="text-uppercase"
            size="small"
            label
          ></v-chip>
        </div>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      search: '',
      items: [
        {
          name: 'Nebula GTX 3080',
          image: '1.png',
          price: 699.99,
          rating: 5,
          stock: true
        },
        {
          name: 'Galaxy RTX 3080',
          image: '2.png',
          price: 799.99,
          rating: 4,
          stock: false
        },
        {
          name: 'Orion RX 6800 XT',
          image: '3.png',
          price: 649.99,
          rating: 3,
          stock: true
        },
        {
          name: 'Vortex RTX 3090',
          image: '4.png',
          price: 1499.99,
          rating: 4,
          stock: true
        },
        {
          name: 'Cosmos GTX 1660 Super',
          image: '5.png',
          price: 299.99,
          rating: 4,
          stock: false
        }
      ]
    };
  }
};
</script>
```

### Stepper

```vue
<template>
  <v-stepper v-model="step" :items="items" show-actions>
    <template v-slot:item.1>
      <h3 class="text-h6">Order</h3>

      <br />

      <v-sheet border>
        <v-table>
          <thead>
            <tr>
              <th>Description</th>
              <th class="text-end">Quantity</th>
              <th class="text-end">Price</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="(product, index) in products" :key="index">
              <td v-text="product.name"></td>
              <td class="text-end" v-text="product.quantity"></td>
              <td
                class="text-end"
                v-text="product.quantity * product.price"
              ></td>
            </tr>

            <tr>
              <th>Total</th>
              <th></th>
              <th class="text-end">${{ subtotal }}</th>
            </tr>
          </tbody>
        </v-table>
      </v-sheet>
    </template>

    <template v-slot:item.2>
      <h3 class="text-h6">Shipping</h3>

      <br />

      <v-radio-group v-model="shipping" label="Delivery Method">
        <v-radio label="Standard Shipping" value="5"></v-radio>
        <v-radio label="Priority Shipping" value="10"></v-radio>
        <v-radio label="Express Shipping" value="15"></v-radio>
      </v-radio-group>
    </template>

    <template v-slot:item.3>
      <h3 class="text-h6">Confirm</h3>

      <br />

      <v-sheet border>
        <v-table>
          <thead>
            <tr>
              <th>Description</th>
              <th class="text-end">Quantity</th>
              <th class="text-end">Price</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="(product, index) in products" :key="index">
              <td v-text="product.name"></td>
              <td class="text-end" v-text="product.quantity"></td>
              <td
                class="text-end"
                v-text="product.quantity * product.price"
              ></td>
            </tr>

            <tr>
              <td>Shipping</td>
              <td></td>
              <td class="text-end" v-text="shipping"></td>
            </tr>

            <tr>
              <th>Total</th>
              <th></th>
              <th class="text-end">${{ total }}</th>
            </tr>
          </tbody>
        </v-table>
      </v-sheet>
    </template>
  </v-stepper>
</template>

<script setup>
import { computed, ref } from 'vue';

const shipping = ref(0);
const step = ref(1);
const subtotal = computed(() =>
  products.reduce((acc, product) => acc + product.quantity * product.price, 0)
);
const total = computed(() => subtotal.value + Number(shipping.value ?? 0));

const items = ['Review Order', 'Select Shipping', 'Submit'];
const products = [
  {
    name: 'Product 1',
    price: 10,
    quantity: 2
  },
  {
    name: 'Product 2',
    price: 15,
    quantity: 10
  }
];
</script>

<script>
export default {
  data: () => ({
    shipping: 0,
    step: 1,
    items: ['Review Order', 'Select Shipping', 'Submit'],
    products: [
      {
        name: 'Product 1',
        price: 10,
        quantity: 2
      },
      {
        name: 'Product 2',
        price: 15,
        quantity: 10
      }
    ]
  }),

  computed: {
    subtotal() {
      return this.products.reduce(
        (acc, product) => acc + product.quantity * product.price,
        0
      );
    },
    total() {
      return this.subtotal + Number(this.shipping ?? 0);
    }
  }
};
</script>
```
