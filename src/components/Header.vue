<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <strong>Quiz</strong>
      </b-nav-item>
      <b-nav-item disabled>Counter : {{score}}/10</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["score"]
};
</script>