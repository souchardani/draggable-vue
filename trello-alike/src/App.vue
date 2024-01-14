<template>
  <div id="app">
    <Header></Header>
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">Todo</h2>
        <Container
          group-name="trello"
          @drag-start="handleDragStart('backlog', $event)"
          @drop="handleDrop('backlog', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }"
        >
          <Draggable v-for="card in cards.backlog" :key="card.id">
            <Card> {{ card.description }}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">In process</h2>
        <Container
          group-name="trello"
          @drag-start="handleDragStart('dev', $event)"
          @drop="handleDrop('dev', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }"
        >
          <Draggable v-for="card in cards.dev" :key="card.id">
            <Card> {{ card.description }}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">Testing</h2>
        <Container
          group-name="trello"
          @drag-start="handleDragStart('testes', $event)"
          @drop="handleDrop('testes', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }"
        >
          <Draggable v-for="card in cards.testes" :key="card.id">
            <Card> {{ card.description }}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">Done</h2>
        <Container
          group-name="trello"
          @drag-start="handleDragStart('fechados', $event)"
          @drop="handleDrop('fechados', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'placeholder' }"
        >
          <Draggable v-for="card in cards.fechados" :key="card.id">
            <Card> {{ card.description }}</Card>
          </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Card from "./components/Card.vue";
import initialCards from "./initialCards";
//libreria para el draggable
import { Container, Draggable } from "vue3-smooth-dnd";
export default {
  name: "App",
  components: { Header, Card, Container, Draggable },
  data() {
    return {
      cards: {
        backlog: initialCards.backlog,
        dev: initialCards.dev,
        testes: initialCards.testes,
        fechados: initialCards.fechados,
      },
      dragginCard: {
        lane: "",
        index: -1,
        cardData: {},
      },
    };
  },
  methods: {
    handleDragStart(lane, dragResult) {
      const { payload, isSource } = dragResult;
      if (isSource) {
        this.dragginCard = {
          lane,
          index: payload.index,
          cardData: {
            ...this.cards[lane][payload.index],
          },
        };
      }
    },
    handleDrop(lane, dropResult) {
      const { removedIndex, addedIndex } = dropResult;
      if (lane === this.dragginCard.lane && removedIndex === addedIndex) {
        return;
      }
      if (removedIndex !== null) {
        this.cards[lane].splice(removedIndex, 1);
      }
      if (addedIndex !== null) {
        this.cards[lane].splice(addedIndex, 0, this.dragginCard.cardData);
      }
    },
    getChildPayload(index) {
      return {
        index,
      };
    },
  },
};
</script>

<style>
.board {
  display: flex;
  justify-content: flex-start;
  margin: 1.2rem 0.8rem;
  align-items: flex-start;
}

.lane {
  background: var(--color-grey);
  width: 23rem;
  border-radius: 0.8rem;
  box-shadow: 0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
  margin: 0 0.8rem;
  padding: 0 0.7rem;
}

.lane-title {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
}

.placeholder {
  background: rgba(33, 33, 33, 0.08);
  border-radius: 0.4rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}
</style>
