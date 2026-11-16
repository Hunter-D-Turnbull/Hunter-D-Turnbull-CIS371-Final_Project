<script setup>
import { ref, computed } from 'vue'

const drawer = ref(true)
const search = ref('')

const navigation = [
  { title: 'Dashboard', icon: 'mdi-view-dashboard', value: 'dashboard' },
  { title: 'My Collection', icon: 'mdi-cards', value: 'collection' },
  { title: 'Global Collection', icon: 'mdi-earth', value: 'global' },
  { title: 'Deck Builder', icon: 'mdi-sword-cross', value: 'decks' },
  { title: 'Public Decks', icon: 'mdi-account-group', value: 'public' },
]

const activePage = ref('dashboard')

const cards = ref([
  {
    name: 'Nissa, Resurgent Animist',
    set: 'MAT',
    quantity: 2,
    color: 'green',
  },
  {
    name: 'Sol Ring',
    set: 'CMM',
    quantity: 3,
    color: 'orange',
  },
  {
    name: 'Command Tower',
    set: 'CMM',
    quantity: 4,
    color: 'blue',
  },
  {
    name: 'Cultivate',
    set: 'M21',
    quantity: 2,
    color: 'green',
  },
])

const filteredCards = computed(() =>
  cards.value.filter(card =>
    card.name.toLowerCase().includes(search.value.toLowerCase())
  )
)

const totalCards = computed(() =>
  cards.value.reduce((total, card) => total + card.quantity, 0)
)
</script>

<template>
  <v-app theme="dark">
    <v-navigation-drawer
      v-model="drawer"
      width="250"
      color="#15171c"
    >
      <div class="pa-5">
        <div class="d-flex align-center ga-3">
          <v-avatar color="#b58b43" rounded="lg" size="42">
            <v-icon icon="mdi-cards" color="black" size="26" />
          </v-avatar>

          <div>
            <div class="text-h6 font-weight-bold">ManaVault</div>
            <div class="text-caption text-medium-emphasis">
              MTG Collection Manager
            </div>
          </div>
        </div>
      </div>

      <v-divider class="mb-3" />

      <v-list nav class="px-3" bg-color="transparent">
        <v-list-item
          v-for="item in navigation"
          :key="item.value"
          :title="item.title"
          :prepend-icon="item.icon"
          :active="activePage === item.value"
          color="amber"
          rounded="lg"
          class="mb-1"
          @click="activePage = item.value"
        />
      </v-list>

      <template #append>
        <v-divider />
        <div class="pa-4 d-flex align-center ga-3">
          <v-avatar color="blue-grey" size="38">
            <v-icon icon="mdi-account" />
          </v-avatar>
          <div>
            <div class="text-body-2 font-weight-bold">Guest Player</div>
            <div class="text-caption text-medium-emphasis">
              Not signed in
            </div>
          </div>
        </div>
      </template>
    </v-navigation-drawer>

    <v-app-bar color="#15171c" elevation="0">
      <v-app-bar-nav-icon @click="drawer = !drawer" />

      <v-toolbar-title class="font-weight-bold">
        {{ navigation.find(item => item.value === activePage)?.title }}
      </v-toolbar-title>

      <v-spacer />

      <v-btn variant="text" icon="mdi-bell-outline" />
      <v-btn color="amber" variant="flat" class="mr-3">
        <v-icon start icon="mdi-login" />
        Sign In
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-container fluid class="pa-6">
        <template v-if="activePage === 'dashboard'">
          <div class="mb-6">
            <div class="text-h4 font-weight-bold mb-2">
              Welcome to ManaVault
            </div>
            <div class="text-body-1 text-medium-emphasis">
              Your Magic: The Gathering collection, all in one place.
            </div>
          </div>

          <v-row class="mb-4">
            <v-col cols="12" sm="6" md="4">
              <v-card class="stat-card pa-5" rounded="lg">
                <div class="d-flex justify-space-between">
                  <div>
                    <div class="text-medium-emphasis text-body-2">
                      Total Cards
                    </div>
                    <div class="text-h3 font-weight-bold mt-2">
                      {{ totalCards }}
                    </div>
                  </div>
                  <v-icon icon="mdi-cards" size="38" color="amber" />
                </div>
              </v-card>
            </v-col>

            <v-col cols="12" sm="6" md="4">
              <v-card class="stat-card pa-5" rounded="lg">
                <div class="d-flex justify-space-between">
                  <div>
                    <div class="text-medium-emphasis text-body-2">
                      Unique Cards
                    </div>
                    <div class="text-h3 font-weight-bold mt-2">
                      {{ cards.length }}
                    </div>
                  </div>
                  <v-icon icon="mdi-layers" size="38" color="green" />
                </div>
              </v-card>
            </v-col>

            <v-col cols="12" sm="6" md="4">
              <v-card class="stat-card pa-5" rounded="lg">
                <div class="d-flex justify-space-between">
                  <div>
                    <div class="text-medium-emphasis text-body-2">
                      My Decks
                    </div>
                    <div class="text-h3 font-weight-bold mt-2">0</div>
                  </div>
                  <v-icon icon="mdi-sword-cross" size="38" color="blue" />
                </div>
              </v-card>
            </v-col>
          </v-row>

          <v-card rounded="lg" class="pa-5 mb-6">
            <div class="d-flex flex-wrap align-center justify-space-between ga-3 mb-4">
              <div>
                <div class="text-h6 font-weight-bold">
                  My Collection
                </div>
                <div class="text-caption text-medium-emphasis">
                  Cards currently in your collection
                </div>
              </div>

              <v-btn color="amber" variant="flat">
                <v-icon start icon="mdi-plus" />
                Add Card
              </v-btn>
            </div>

            <v-text-field
              v-model="search"
              label="Search your cards"
              prepend-inner-icon="mdi-magnify"
              variant="outlined"
              density="comfortable"
              hide-details
              clearable
              class="mb-5"
            />

            <v-row>
              <v-col
                v-for="card in filteredCards"
                :key="card.name"
                cols="12"
                sm="6"
                md="3"
              >
                <v-card class="card-item" rounded="lg">
                  <div
                    class="card-placeholder"
                    :class="card.color"
                  >
                    <v-icon icon="mdi-cards" size="54" />
                    <div class="text-subtitle-1 font-weight-bold mt-2">
                      {{ card.name }}
                    </div>
                    <div class="text-caption mt-1">
                      {{ card.set }}
                    </div>
                  </div>

                  <v-card-text>
                    <div class="font-weight-bold">
                      {{ card.name }}
                    </div>
                    <div class="d-flex justify-space-between align-center mt-3">
                      <v-chip size="small" color="amber">
                        {{ card.quantity }} owned
                      </v-chip>
                      <span class="text-caption text-medium-emphasis">
                        {{ card.set }}
                      </span>
                    </div>
                  </v-card-text>
                </v-card>
              </v-col>

              <v-col v-if="filteredCards.length === 0" cols="12">
                <div class="text-center text-medium-emphasis pa-8">
                  No cards found.
                </div>
              </v-col>
            </v-row>
          </v-card>
        </template>

        <v-card v-else rounded="lg" class="pa-8 text-center">
          <v-icon icon="mdi-hammer-wrench" size="50" color="amber" />
          <div class="text-h5 font-weight-bold mt-4">
            {{ navigation.find(item => item.value === activePage)?.title }}
          </div>
          <div class="text-medium-emphasis mt-2">
            This section is coming soon!
          </div>
        </v-card>

      </v-container>
    </v-main>
  </v-app>
</template>

<style>
body {
  background: #101116;
}

.stat-card {
  background: #1b1e25 !important;
  border: 1px solid #30333b;
}

.card-item {
  background: #1b1e25 !important;
  border: 1px solid #30333b;
  transition: transform 0.2s ease;
}

.card-item:hover {
  transform: translateY(-4px);
}

.card-placeholder {
  height: 210px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: white;
}

.card-placeholder.green {
  background: linear-gradient(145deg, #315c42, #172d23);
}

.card-placeholder.orange {
  background: linear-gradient(145deg, #a46b35, #392519);
}

.card-placeholder.blue {
  background: linear-gradient(145deg, #355d82, #192b40);
}
</style>
