<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Count: {{ numCorrect }} / {{ totalQues }} </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect', 'totalQues'
  ]
}
</script>