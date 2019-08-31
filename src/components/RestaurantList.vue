<template>
  <div class="hello">
    <form @submit.prevent="createRestaurant">
      <input v-model="newRestaurantName" />
      <button>Add</button>
    </form>
    <ul>
      <li v-for="restaurant in restaurants" :key="restaurant.name">
        {{ restaurant.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "RestaurantList",
  data() {
    return {
      newRestaurantName: ""
    };
  },
  methods: {
    createRestaurant() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation($name: String!) {
              createRestaurant(name: $name) {
                name
              }
            }
          `,
          variables: {
            name: this.newRestaurantName
          }
        })
        .then(() => {
          this.newRestaurantName = "";
        });
    }
  },
  apollo: {
    restaurants: {
      query: gql`
        query {
          restaurants {
            name
          }
        }
      `,
      subscribeToMore: {
        document: gql`
          subscription restaurantAdded {
            restaurantAdded {
              name
            }
          }
        `,
        updateQuery: (previousResult, { subscriptionData }) => {
          console.log({ previousResult, subscriptionData });
          const newRestaurant = subscriptionData.data.restaurantAdded;
          return {
            restaurants: [...previousResult.restaurants, newRestaurant]
          };
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
