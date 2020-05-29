<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card>
          <v-data-table
            :headers="table.headers"
            :items="table.items"
            :items-per-page="table.itemsPerPage"
          />
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card>
          <v-card-text>
            <pre>{{ table }}</pre>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import {
  defineComponent,
  reactive,
  onBeforeUnmount
} from '@vue/composition-api'

export default defineComponent({
  setup(_, { root: { $firebase } }) {
    let unsubscribe = (() => {}) as Function

    onBeforeUnmount(() => {
      unsubscribe()
    })

    const table = reactive({
      headers: [
        {
          text: 'Alphabet',
          value: 'alphabet'
        },
        {
          text: 'Code',
          value: 'code'
        }
      ],
      items: [] as { alphabet: string; code: string }[],
      itemsPerPage: 5
    })

    const exRef = $firebase
      .firestore()
      .collection('example')
      .orderBy('alphabet')

    interface IUpdateSnapshotParams {
      startAfter?: string
      endBefore?: string
    }

    const updateSnapshot = (params?: IUpdateSnapshotParams) => {
      let ref = exRef
      ref = ref.limit(table.itemsPerPage)
      if (typeof params?.startAfter !== 'undefined') {
        ref.startAfter(params.startAfter)
      }
      if (typeof params?.endBefore !== 'undefined') {
        ref.endBefore(params.endBefore)
      }

      unsubscribe()
      unsubscribe = ref
        .limit(table.itemsPerPage)
        .onSnapshot((exQuerySnapshot) => {
          table.items = exQuerySnapshot.docs.map((exQueryDocSnapshot) => {
            const exQueryDocData = exQueryDocSnapshot.data()
            return {
              alphabet: exQueryDocData.alphabet,
              code: exQueryDocData.code
            }
          })
        })
    }
    updateSnapshot()

    return {
      table
    }
  }
})
</script>
