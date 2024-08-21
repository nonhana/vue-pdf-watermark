<template>
  <div class="float-example">
    <img
      src="https://via.placeholder.com/150"
      alt="Placeholder Image"
      class="float-img"
    />
    <p>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum vitae
      neque mauris. Pellentesque habitant morbi tristique senectus et netus et
      malesuada fames ac turpis egestas. Phasellus scelerisque, mauris a
      accumsan hendrerit, lectus neque dignissim nisi, a sodales enim diam ut
      risus. Proin id interdum leo. Morbi mollis, odio et elementum efficitur,
      enim odio auctor libero, in dignissim nunc sapien quis dui. Fusce id
      volutpat nulla. Curabitur vehicula, lorem nec vestibulum auctor, magna
      diam vehicula enim, sed pellentesque arcu velit in risus. Cras tincidunt
      eros vel ipsum faucibus, ac posuere dolor facilisis. Sed nec ex nec turpis
      aliquam fermentum. Sed lacinia nunc non magna fermentum, non consectetur
      dui tincidunt.
    </p>
  </div>
</template>

<script setup lang="ts"></script>

<style>
.float-img {
  float: left; /* Set the image to float left */
  margin-right: 20px; /* Add some space between the text and the image */
}
.float-example {
  width: 500px; /* Set the width of the container */
  border: solid;
  clear: both; /* Clear the float */
}
.float-example p {
  overflow: hidden; /* Ensures text wraps around the floating image */
}
</style>
