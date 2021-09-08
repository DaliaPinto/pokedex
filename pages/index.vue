<template>
    <el-container
        direction="horizontal"
    >
        <el-header>
            <img
                src="~/static/logo.png"
                class="logo"
            >
        </el-header>
        <el-main>
            <ItemsCard />
        </el-main>

    </el-container>
</template>

<script>
    import ItemsCard from '@/components/ItemsCard'
    export default {
        name: 'main-item',
        components: {
            ItemsCard
        }
    }
</script>

<style>
    </style>
