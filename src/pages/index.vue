<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card>
          <v-card-title>
            Hello World
          </v-card-title>
          <v-card-text>
            aaaa
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  setup(_, { root: { $firebase, $router } }) {
    const exampleRef = $firebase.firestore().collection('example')

    exampleRef.get().then((exampleQuerySnapshot) => {
      if (exampleQuerySnapshot.empty) {
        $router.push('/init')
      }
    })
  }
})
</script>
