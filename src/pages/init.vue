<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  setup(_, { root: { $firebase, $router } }) {
    const exampleRef = $firebase.firestore().collection('example')
    const exampleDataList = [
      { alphabet: 'a', code: 'alpha' },
      { alphabet: 'b', code: 'bravo' },
      { alphabet: 'c', code: 'charlie' },
      { alphabet: 'd', code: 'delta' },
      { alphabet: 'e', code: 'echo' },
      { alphabet: 'f', code: 'foxtrot' },
      { alphabet: 'g', code: 'golf' },
      { alphabet: 'h', code: 'hotel' },
      { alphabet: 'i', code: 'india' },
      { alphabet: 'j', code: 'juliet' },
      { alphabet: 'k', code: 'kilo' },
      { alphabet: 'l', code: 'lima' },
      { alphabet: 'm', code: 'mike' },
      { alphabet: 'n', code: 'november' },
      { alphabet: 'o', code: 'oscar' },
      { alphabet: 'p', code: 'papa' },
      { alphabet: 'q', code: 'quebec' },
      { alphabet: 'r', code: 'romeo' },
      { alphabet: 's', code: 'sierra' },
      { alphabet: 't', code: 'tango' },
      { alphabet: 'u', code: 'uniform' },
      { alphabet: 'v', code: 'victor' },
      { alphabet: 'w', code: 'whiskey' },
      { alphabet: 'x', code: 'x-ray' },
      { alphabet: 'y', code: 'yankee' },
      { alphabet: 'z', code: 'zulu' }
    ]

    exampleRef.get().then((exampleQuerySnapshot) => {
      if (exampleQuerySnapshot.empty) {
        $router.push('/init')
      }
    })

    exampleDataList.forEach((exampleData) => {
      exampleRef.add(exampleData)
    })

    $router.push('/')
  }
})
</script>
