<template>
    <section class="header">
        <div class="container">
            <div class="header__inner">
                <slot></slot>
                <h3><i>Pokemon World!</i></h3>
            </div>
        </div>
    </section>
</template>

<script setup>

</script>

<style lang="scss">
@import "/src/assets/styles/style.scss";
@import "/src/assets/styles/_variables.scss";

.header {
    background: $color1;
    padding: 20px 0px;
    color: $white;
    text-align: center;
    position: relative;

    .container {
        position: relative;
    }

    h3 {
        margin: 0px;
    }
}
</style>