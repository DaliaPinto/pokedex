<template>
    <el-row
        class="mt-2"
    >
        <el-card>
            <el-row>
                <el-col
                    :span="8"
                    :offset="16"
                >
                    <el-input
                        class="right"
                        v-model="search"
                        placeholder="Type your favorite pokemon"
                        prefix-icon="el-icon-search"
                        size="small"
                    />
                </el-col>
            </el-row>

            <el-divider></el-divider>

            <el-row
                :gutter="24"
                v-loading="loading"
                class="mb-4 mt-4"
            >
                <el-col
                    :span="6"
                    v-for="(pokemon, index) in pokemons.filter(item => !search || item.name.includes(search))"
                    :key="index"
                    class="mb-3"
                >
                    <ItemProperties
                        :pokemon-name="pokemon.name"
                    />
                </el-col>
            </el-row>

            <el-row
                class="mt-3"
            >
                <el-pagination
                    layout="prev, pager, next"
                    :total="total"
                    @current-change="getPokemons"
                    class="pagination"
                >
                </el-pagination>
            </el-row>
        </el-card>
    </el-row>
</template>

<script>
    import ItemProperties from '@/components/ItemProperties'
    export default {
        name: 'items-card-component',
        components: {
            ItemProperties
        },
        data() {
            return {
                title: '',
                pokemons: [],
                params: {
                    limit: 20,
                    offset: 0
                },
                loading: false,
                total: 0,
                search: ''
            }
        },
        methods: {
            getPokemons(page) {
                this.params.offset = this.params.limit * (page - 1)
                this.loading = true

                this.$axios.get('https://pokeapi.co/api/v2/pokemon/', {
                    params: this.params
                })
                .then(response => {
                    let data = response.data

                    this.total = data.count
                    this.loading = false

                    this.pokemons = data.results
                }
            )},
        },
        mounted() {
            this.getPokemons(1)
        }
    }
</script>
