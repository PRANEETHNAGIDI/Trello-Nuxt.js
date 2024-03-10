<template>
    <div :class="{ 'dark-bg': isDarkMode,'light-bg': !isDarkMode }"  class="flex1 flex-col  h-screen ">
      <header class="shrink-0 flex justify-between bg-white px-4 py-3">
        <a class="text-2xl font-black" href="/">Trello</a>
        <nav>
            <button @click="toggleDarkMode" class="dark-mode-btn">Dark Mode</button>

        </nav>
      </header>
      <main class="flex-1 ">
        <div class="flex flex-col h-full">
          <!-- Boards Container -->
          <div class="flex flex-wrap justify-around ">
            <!-- Backlog Board -->
            <div class="board-container bg-gray-200 rounded-md bg-gray-200">
              <div class="board-header flex justify-between ">
                <h1 class="text-2xl text-black font-bold">Backlog</h1>
                <div class="card-count">{{ backlogCards.length }}</div>
      
              </div>
              <draggable v-model="backlogCards" class="card-container" group="cards">
                <template #item="{element, index}">
                  <div :key="'backlog_' + index" class="card ">
                    <a href="#"  @click="displayCardDetails(element, 'Backlog')">
                      {{ element.title }}
                    </a>
                  </div>
                </template>
              </draggable>
              <div class="add-card ">
                <CardListItemCreateForm @cardAdded="handleCardAdded('Backlog', $event)" />
              </div>
            </div>
            <!-- Not Started Board -->
            <div class="board-container bg-gray-200 rounded-md">
              <div class="board-header">
                <h1 class="text-2xl text-black font-bold">Not Started</h1>
                <div class="card-count">{{ notStartedCards.length }}</div>

              </div>
              <draggable v-model="notStartedCards" class="card-container" group="cards">
                <template #item="{element, index}">
                  <div :key="'not_started_' + index" class="card">
                    <a href="#" @click="displayCardDetails(element, 'Not Started')">
                      {{ element.title }}
                    </a>
                  </div>
                </template>
              </draggable>
              <div class="add-card">
                <CardListItemCreateForm @cardAdded="handleCardAdded('Not Started', $event)" />
              </div>
            </div>
            <!-- In Progress Board -->
            <div class="board-container bg-gray-200 rounded-md">
              <div class="board-header">
                <h1 class="text-2xl text-black font-bold">In Progress</h1>
                <div class="card-count">{{ inProgressCards.length }}</div>
      
              </div>
              <draggable v-model="inProgressCards" class="card-container" group="cards">
                <template #item="{element, index}">
                  <div :key="'in_progress_' + index" class="card">
                    <a href="#" @click="displayCardDetails(element, 'In Progress')">
                      {{ element.title }}
                    </a>
                  </div>
                </template>
              </draggable>
              <div class="add-card">
                <CardListItemCreateForm @cardAdded="handleCardAdded('In Progress', $event)" />
              </div>
            </div>
            <!-- Completed Board -->
            <div class="board-container bg-gray-200 rounded-md">
              <div class="board-header">
                <h1 class="text-2xl text-black font-bold">Completed</h1>
                <div class="card-count">{{ completedCards.length }}</div>
      
              </div>
              <draggable v-model="completedCards" class="card-container" group="cards">
                <template #item="{element, index}">
                  <div :key="'completed_' + index" class="card">
                    <a href="#" @click="displayCardDetails(element, 'Completed')">
                      {{ element.title }}
                    </a>
                  </div>
                </template>
              </draggable>
              <div class="add-card">
                <CardListItemCreateForm @cardAdded="handleCardAdded('Completed', $event)" />
              </div>
            </div>
          </div>
          <!-- Card Details -->
          <div v-if="selectedCard" class="overlay">
            <div class="card-details">
              <h2 class="text-2xl font-bold mb-4">{{ selectedCard.title }}</h2>
              <p>{{ selectedCard.description }}</p>
              <button @click="deleteSelectedCard" class="mt-4 bg-red-500 text-white px-4 py-2 rounded-md">Delete</button>
            </div>
          </div>
        </div>
      </main>
    </div>
  </template>
  
  <script setup>
  import CardListItemCreateForm from "@/pages/CardListItemCreateForm";
  import draggable from 'vuedraggable';
  import { ref } from "vue";
  
  // Reactive variables for each board
  const backlogCards = ref([
    { title: 'Backlog Card 1', description: 'Description for Backlog Card 1' },
    { title: 'Backlog Card 2', description: 'Description for Backlog Card 2' },
    { title: 'Backlog Card 3', description: 'Description for Backlog Card 3' },
  ]);
  
  const notStartedCards = ref([
    { title: 'Not Started Card 1', description: 'Description for Not Started Card 1' },
    { title: 'Not Started Card 2', description: 'Description for Not Started Card 2' },
    { title: 'Not Started Card 3', description: 'Description for Not Started Card 3' },
  ]);
  
  const inProgressCards = ref([
    { title: 'In Progress Card 1', description: 'Description for In Progress Card 1' },
    { title: 'In Progress Card 2', description: 'Description for In Progress Card 2' },
    { title: 'In Progress Card 3', description: 'Description for In Progress Card 3' },
  ]);
  
  const completedCards = ref([
    { title: 'Completed Card 1', description: 'Description for Completed Card 1' },
    { title: 'Completed Card 2', description: 'Description for Completed Card 2' },
    { title: 'Completed Card 3', description: 'Description for Completed Card 3' },
  ]);
  
  const selectedCard = ref(null);
  const selectedBoard = ref(null);
  
  // Methods to handle card operations for each board
  const handleCardAdded = (board, newCard) => {
    if (board === 'Backlog') {
      backlogCards.value.push(newCard);
    } else if (board === 'Not Started') {
      notStartedCards.value.push(newCard);
    } else if (board === 'In Progress') {
      inProgressCards.value.push(newCard);
    } else if (board === 'Completed') {
      completedCards.value.push(newCard);
    }
  };
  
  const displayCardDetails = (card, board) => {
    selectedCard.value = card;
    selectedBoard.value = board;
  };
  
  const deleteSelectedCard = () => {
    const board = selectedBoard.value;
    let cards;
    if (board === 'Backlog') {
      cards = backlogCards;
    } else if (board === 'Not Started') {
      cards = notStartedCards;
    } else if (board === 'In Progress') {
      cards = inProgressCards;
    } else if (board === 'Completed') {
      cards = completedCards;
    }
    
    const index = cards.value.indexOf(selectedCard.value);
    if (index !== -1) {
      cards.value.splice(index, 1);
    }
    selectedCard.value = null;
  };
  const isDarkMode = ref(false);

const toggleDarkMode = () => {
  isDarkMode.value = !isDarkMode.value;
  if (isDarkMode.value) {
    document.documentElement.classList.add('dark');
  } else {
    document.documentElement.classList.remove('dark');
  }
};

  </script>
  
  <style scoped>
  
  .board-container {
    margin: 20px;
    flex: 1 1 300px; 
    min-width: 250px;
  }
  
  .board-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 10px;
  }
  
  .card-container {
    display: flex;
    flex-direction: column;
    gap: 10px; 
  }
  
  .card {
    background-color: #fff;
    padding: 10px;
    margin: 3px 15px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 10;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .card-details {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .add-card {
    padding: 10px;
  }
  
 
  .dark-bg {
    background-color: #1B262C;
  }.light-bg {
  background-color: #0f4c75; 
}



  @media (max-width: 768px) {
    .board-container {
      flex-basis: calc(50% - 40px);
    }
  }
  </style>
  