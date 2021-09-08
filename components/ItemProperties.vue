<template>
    <div>
        <el-card
            shadow="hover"
            class="hover"
        >
            <div
                @click="getPokemonDetails"
            >
              <img
                  :src="selected_pokemon.sprites ? selected_pokemon.sprites.front_default : ''"
                  class="image"
              >
              <el-row
                  :gutter="24"
              >
                  <el-col
                      :span="12"
                  >
                      <span
                          class="name"
                      >
                          {{ selected_pokemon.name }}
                      </span>
                  </el-col>

                  <el-col
                      :span="12"
                  >
                    <el-tag
                        v-for="(type, index) in selected_pokemon.types"
                        :key="index"
                        :type="types[type.type.name].color"
                        size="mini"
                        class="right mr-1"
                    >
                        {{ type.type.name }}
                    </el-tag>
                  </el-col>
              </el-row>
              <el-row
                  :gutter="24"
                  class="mt-1"
              >
                  <el-col
                      :span="12"
                  >
                      <span
                          class="details"
                      >
                          <strong> Position: </strong>{{ selected_pokemon.order }}
                      </span>
                  </el-col>
                  <el-col
                      :span="12"
                  >
                      <span
                          class="details right"
                      >
                          <strong> Weight: </strong> {{ selected_pokemon.weight }}
                      </span>
                  </el-col>
              </el-row>
            </div>
        </el-card>

        <!--modal -->
        <el-dialog
            :visible.sync="visible"
            append-to-body
        >
            <span
                slot="title"
                class="name"
            >
                {{ selected_pokemon.order }} - {{ selected_pokemon.name }}
            </span>
            <el-row
                :gutter="24"
            >
                <el-col
                    :span="6"
                >
                    <img
                        :src="selected_pokemon.sprites ? selected_pokemon.sprites.front_default : ''"
                        style="vertical-align: middle;"
                    />
                    <img
                        :src="selected_pokemon.sprites ? selected_pokemon.sprites.back_default : ''"
                    />
                </el-col>

                <el-col
                    :span="18"
                >
                    <el-descriptions
                        title="Stats"
                        :column="3"
                        size="mini"
                        class="mt-5"
                        border
                    >
                        <el-descriptions-item
                            v-for="(stat, index) in selected_pokemon.stats"
                            :key="index"
                            :label="stat.stat.name"
                        >
                            {{ stat.base_stat }}
                        </el-descriptions-item>

                        <el-descriptions-item
                            label="Total"
                        >
                            {{ total_stats }}
                        </el-descriptions-item>
                    </el-descriptions>
                </el-col>
            </el-row>

            <div
                slot="footer"
            >
            </div>
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'item-properties-component',
    props: {
        pokemonName: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            selected_pokemon: {},
            visible: false,
            total_stats: 0
        }
    },
    methods: {
        getPokemonDetails() {
            this.visible = true
        },
        showPokemon() {
            this.$axios.get('https://pokeapi.co/api/v2/pokemon/' + this.pokemonName)
            .then(response => {
                this.selected_pokemon = response.data
                this.getTotalStats()
            })
        },
        getTotalStats() {
            let total = 0

            this.selected_pokemon.stats.forEach(function (stat) {
                total+=stat.base_stat
            })

            this.total_stats = total
        }
    },
    computed: {
        types() {
            return {
                normal: {
                    color: 'info'
                },
                fighting: {
                    color: 'danger'
                },
                flying: {
                    color: ''
                },
                poison: {
                    color: 'success'
                },
                ground: {
                    color: 'warning'
                },
                rock: {
                    color: 'warning'
                },
                bug: {
                    color: 'success'
                },
                ghost: {
                    color: ''
                },
                steel: {
                    color: 'info'
                },
                fire: {
                    color: 'danger'
                },
                water: {
                    color: ''
                },
                grass: {
                    color: 'success'
                },
                electric: {
                    color: 'warning'
                },
                psychic: {
                    color: 'danger'
                },
                ice: {
                    color: ''
                },
                dragon: {
                    color: 'success'
                },
                dark: {
                    color: 'info'
                },
                fairy: {
                    color: 'warning'
                }
            }
        }
    },
    watch: {
        pokemonName(value) {
            this.showPokemon()
        }
    },
    mounted() {
        this.showPokemon()
    },
}
</script>

<style scoped>
    .image {
        /*width: 100%;*/
        margin: auto;
        display: block;
    }

    .name {
        color:#F56C6C;
        font-size: 16px;
        font-weight: 700;
        text-transform:capitalize;

    }

    .hover {
        cursor: pointer;
    }

    .details {
        color:#909399;
        font-size: 12px;

    }
</style>
