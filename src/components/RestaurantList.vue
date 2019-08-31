<template>
  <div class="hello">
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
