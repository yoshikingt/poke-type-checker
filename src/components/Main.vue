<template>
  <b-container>
    <b-row>
      <b-col>
        <h2>タイプ相性チェッカー</h2>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <div>相手ポケモンのタイプを選択してください</div>
        <div>単タイプの場合は右側を「なし」にしてください</div>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <b-form-select
          v-model="selectedValue1"
          v-bind:options="typeList"
          value-field="id"
          text-field="name"
          v-on:change="effectJudgment()"
        >
        </b-form-select>
      </b-col>
      <b-col>
        <b-form-select
          v-model="selectedValue2"
          v-bind:options="typeList"
          value-field="id"
          text-field="name"
          v-on:change="effectJudgment()"
        >
        </b-form-select>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <b-button variant="outline-primary" v-on:click="clear()">
          クリア
        </b-button>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        <b-table-simple>
          <b-tbody>
            <b-tr>
              <b-th>4倍弱点</b-th>
              <b-td>
                <div
                  v-for="type of verySuperEfctTypeList"
                  v-bind:key="type.getId"
                  class="type-style"
                  v-bind:class="type.getId"
                >
                  {{ type.getName }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>2倍弱点</b-th>
              <b-td>
                <div
                  v-for="type of superEfctTypeList"
                  v-bind:key="type.getId"
                  class="type-style"
                  v-bind:class="type.getId"
                >
                  {{ type.getName }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>1/2耐性</b-th>
              <b-td>
                <div
                  v-for="type of notVeryEfctTypeList"
                  v-bind:key="type.getId"
                  class="type-style"
                  v-bind:class="type.getId"
                >
                  {{ type.getName }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>1/4耐性</b-th>
              <b-td>
                <div
                  v-for="type of notVerySuperEfctTypeList"
                  v-bind:key="type.getId"
                  class="type-style"
                  v-bind:class="type.getId"
                >
                  {{ type.getName }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>こうかなし</b-th>
              <b-td>
                <div
                  v-for="type of nonEfctTypeList"
                  v-bind:key="type.getId"
                  class="type-style"
                  v-bind:class="type.getId"
                >
                  {{ type.getName }}
                </div>
              </b-td>
            </b-tr>
          </b-tbody>
        </b-table-simple>
      </b-col>
    </b-row>
    <b-row>
      <b-col><small>Copyright &copy; 2020 poke-type-checker</small></b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Main extends Vue {
  /** data */
  private readonly NORMAL: Type = new Type(
    "normal",
    "ノーマル",
    ["fighting"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "poison",
      "ground",
      "flying",
      "psychic",
      "bug",
      "rock",
      "dragon",
      "dark",
      "steel",
      "fairy"
    ],
    [""],
    ["ghost"]
  );
  private readonly FIRE: Type = new Type(
    "fire",
    "ほのお",
    ["water", "ground", "rock"],
    [
      "normal",
      "erectric",
      "fighting",
      "poison",
      "flying",
      "psychic",
      "ghost",
      "dragon",
      "dark"
    ],
    ["fire", "grass", "ice", "bug", "steel", "fairy"],
    []
  );
  private readonly WATER: Type = new Type(
    "water",
    "みず",
    ["erectric", "grass"],
    [
      "normal",
      "fighting",
      "poison",
      "ground",
      "flying",
      "psychic",
      "bug",
      "rock",
      "ghost",
      "dragon",
      "dark",
      "fairy"
    ],
    ["fire", "water", "ice", "steel"],
    []
  );
  private readonly ERECTRIC: Type = new Type(
    "erectric",
    "でんき",
    ["ground"],
    [
      "normal",
      "fire",
      "water",
      "grass",
      "ice",
      "fighting",
      "poison",
      "psychic",
      "bug",
      "rock",
      "ghost",
      "dragon",
      "dark",
      "fairy"
    ],
    ["erectric", "flying", "steel"],
    []
  );
  private readonly GRASS: Type = new Type(
    "grass",
    "くさ",
    ["fire", "ice", "poison", "flying", "bug"],
    [
      "normal",
      "fighting",
      "psychic",
      "rock",
      "ghost",
      "dragon",
      "dark",
      "steel",
      "fairy"
    ],
    ["water", "erectric", "grass", "ground"],
    []
  );
  private readonly ICE: Type = new Type(
    "ice",
    "こおり",
    ["fire", "fighting", "rock", "steel"],
    [
      "normal",
      "water",
      "erectric",
      "grass",
      "poison",
      "ground",
      "flying",
      "psychic",
      "bug",
      "ghost",
      "dragon",
      "dark",
      "fairy"
    ],
    ["ice"],
    []
  );
  private readonly FIGHTING: Type = new Type(
    "fighting",
    "かくとう",
    ["flying", "psychic", "fairy"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "fighting",
      "poison",
      "ground",
      "ghost",
      "dragon",
      "steel"
    ],
    ["bug", "rock", "dark"],
    []
  );
  private readonly POISON: Type = new Type(
    "poison",
    "どく",
    ["ground", "psychic"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "ice",
      "flying",
      "rock",
      "ghost",
      "dragon",
      "dark",
      "steel"
    ],
    ["grass", "fighting", "poison", "bug", "fairy"],
    []
  );
  private readonly GROUND: Type = new Type(
    "ground",
    "じめん",
    ["water", "grass", "ice"],
    [
      "normal",
      "fire",
      "fighting",
      "ground",
      "flying",
      "psychic",
      "bug",
      "ghost",
      "dragon",
      "dark",
      "steel",
      "fairy"
    ],
    ["poison", "rock"],
    ["erectric"]
  );
  private readonly FLYING: Type = new Type(
    "flying",
    "ひこう",
    ["erectric", "ice", "rock"],
    [
      "normal",
      "fire",
      "water",
      "poison",
      "flying",
      "psychic",
      "ghost",
      "dragon",
      "dark",
      "steel",
      "fairy"
    ],
    ["grass", "fighting", "bug"],
    ["ground"]
  );
  private readonly PSYCHIC: Type = new Type(
    "psychic",
    "エスパー",
    ["bug", "ghost", "dark"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "poison",
      "ground",
      "flying",
      "rock",
      "dragon",
      "steel",
      "fairy"
    ],
    ["fighting", "psychic"],
    []
  );
  private readonly BUG: Type = new Type(
    "bug",
    "むし",
    ["fire", "flying", "rock"],
    [
      "normal",
      "water",
      "erectric",
      "ice",
      "poison",
      "psychic",
      "bug",
      "ghost",
      "dragon",
      "dark",
      "steel",
      "fairy"
    ],
    ["grass", "fighting", "ground"],
    []
  );
  private readonly ROCK: Type = new Type(
    "rock",
    "いわ",
    ["water", "grass", "fighting", "ground", "steel"],
    [
      "erectric",
      "ice",
      "psychic",
      "bug",
      "rock",
      "ghost",
      "dragon",
      "dark",
      "fairy"
    ],
    ["normal", "fire", "poison", "flying"],
    []
  );
  private readonly GHOST: Type = new Type(
    "ghost",
    "ゴースト",
    ["ghost", "dark"],
    [
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "ground",
      "flying",
      "psychic",
      "rock",
      "dragon",
      "steel",
      "fairy"
    ],
    ["poison", "bug"],
    ["normal", "fighting"]
  );
  private readonly DRAGON: Type = new Type(
    "dragon",
    "ドラゴン",
    ["ice", "dragon", "fairy"],
    [
      "normal",
      "fighting",
      "poison",
      "ground",
      "flying",
      "psychic",
      "bug",
      "rock",
      "ghost",
      "dark",
      "steel"
    ],
    ["fire", "water", "erectric", "grass"],
    []
  );
  private readonly DARK: Type = new Type(
    "dark",
    "あく",
    ["fighting", "bug", "fairy"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "poison",
      "ground",
      "flying",
      "rock",
      "dragon",
      "steel"
    ],
    ["ghost", "dark"],
    ["psychic"]
  );
  private readonly STEEL: Type = new Type(
    "steel",
    "はがね",
    ["fire", "fighting", "ground"],
    ["water", "erectric", "ghost", "dark"],
    [
      "normal",
      "grass",
      "ice",
      "flying",
      "psychic",
      "bug",
      "rock",
      "dragon",
      "steel",
      "fairy"
    ],
    ["poison"]
  );
  private readonly FAIRY: Type = new Type(
    "fairy",
    "フェアリー",
    ["poison", "steel"],
    [
      "normal",
      "fire",
      "water",
      "erectric",
      "grass",
      "ice",
      "ground",
      "flying",
      "psychic",
      "rock",
      "ghost",
      "fairy"
    ],
    ["fighting", "bug", "dark"],
    ["dragon"]
  );
  private typeList: ReadonlyArray<Type> = [
    new Type("none", "なし", [], [], [], []),
    this.NORMAL,
    this.FIRE,
    this.WATER,
    this.ERECTRIC,
    this.GRASS,
    this.ICE,
    this.FIGHTING,
    this.POISON,
    this.GROUND,
    this.FLYING,
    this.PSYCHIC,
    this.BUG,
    this.ROCK,
    this.GHOST,
    this.DRAGON,
    this.DARK,
    this.STEEL,
    this.FAIRY
  ];
  private verySuperEfctTypeList: Array<Type> = [];
  private superEfctTypeList: Array<Type> = [];
  private normalEfctTypeList: Array<Type> = [];
  private notVeryEfctTypeList: Array<Type> = [];
  private notVerySuperEfctTypeList: Array<Type> = [];
  private nonEfctTypeList: Array<Type> = [];
  private selectedValue1: string = "none";
  private selectedValue2: string = "none";

  /** computed */
  public get selectedType1(): Type {
    for (const type of this.typeList) {
      if (type.getId === this.selectedValue1) {
        return type;
      }
    }
    return this.typeList[0];
  }

  public get selectedType2(): Type {
    for (const type of this.typeList) {
      if (type.getId === this.selectedValue2) {
        return type;
      }
    }
    return this.typeList[0];
  }

  /** methods */
  public effectJudgment(): void {
    const list1: Array<string> = this.selectedType1.getSuperEfctList;
    const list2: Array<string> = this.selectedType2.getSuperEfctList;
    const list3: Array<string> = this.selectedType1.getNormalEfctList;
    const list4: Array<string> = this.selectedType2.getNormalEfctList;
    const list5: Array<string> = this.selectedType1.getNotVeryEfctList;
    const list6: Array<string> = this.selectedType2.getNotVeryEfctList;
    const list7: Array<string> = this.selectedType1.getNonEfctList;
    const list8: Array<string> = this.selectedType2.getNonEfctList;

    var verySuperEfctList: Array<string> = [];
    var superEfctList: Array<string> = [];
    var normalEfctList: Array<string> = [];
    var notVeryEfctList: Array<string> = [];
    var notVerySuperEfctList: Array<string> = [];
    var nonEfctList: Array<string> = [];
    var verySuperEfctTypeList: Array<Type> = [];
    var superEfctTypeList: Array<Type> = [];
    var normalEfctTypeList: Array<Type> = [];
    var notVeryEfctTypeList: Array<Type> = [];
    var notVerySuperEfctTypeList: Array<Type> = [];
    var nonEfctTypeList: Array<Type> = [];

    const isSingleType: boolean =
      this.selectedType2.getId === "none" ||
      this.selectedType1.getId === this.selectedType2.getId;

    if (isSingleType) {
      // 単タイプなら定義をそのまま表示
      superEfctList = list1;
      normalEfctList = list3;
      notVeryEfctList = list5;
      nonEfctList = list7;
    } else {
      // 複合タイプなら 2 つのタイプ相性から判定して表示
      for (const value of list1) {
        // ばつぐん * ばつぐん
        if (list2.includes(value)) {
          verySuperEfctList.push(value);
        }
        // ばつぐん * とうばい
        if (list4.includes(value)) {
          superEfctList.push(value);
        }
        // ばつぐん * いまひとつ
        if (list6.includes(value)) {
          normalEfctList.push(value);
        }
        // ばつぐん * こうかなし
        if (list8.includes(value)) {
          nonEfctList.push(value);
        }
      }

      for (const value of list3) {
        // とうばい * ばつぐん
        if (list2.includes(value)) {
          superEfctList.push(value);
        }
        // とうばい * とうばい
        if (list4.includes(value)) {
          normalEfctList.push(value);
        }
        // とうばい * いまひとつ
        if (list6.includes(value)) {
          notVeryEfctList.push(value);
        }
        // とうばい * こうかなし
        if (list8.includes(value)) {
          nonEfctList.push(value);
        }
      }

      for (const value of list5) {
        // いまひとつ * ばつぐん
        if (list2.includes(value)) {
          normalEfctList.push(value);
        }
        // いまひとつ * とうばい
        if (list4.includes(value)) {
          notVeryEfctList.push(value);
        }
        // いまひとつ * いまひとつ
        if (list6.includes(value)) {
          notVerySuperEfctList.push(value);
        }
        // いまひとつ * こうかなし
        if (list8.includes(value)) {
          nonEfctList.push(value);
        }
      }

      for (const value of list7) {
        // こうかなし * ばつぐん
        if (list2.includes(value)) {
          nonEfctList.push(value);
        }
        // こうかなし * とうばい
        if (list4.includes(value)) {
          nonEfctList.push(value);
        }
        // こうかなし * いまひとつ
        if (list6.includes(value)) {
          nonEfctList.push(value);
        }
        // こうかなし * こうかなし
        if (list8.includes(value)) {
          nonEfctList.push(value);
        }
      }
    }

    // 判定結果から Type オブジェクトを取得する
    for (const type of this.typeList) {
      for (const str of verySuperEfctList) {
        if (str === type.getId) {
          verySuperEfctTypeList.push(type);
        }
      }
      for (const str of superEfctList) {
        if (str === type.getId) {
          superEfctTypeList.push(type);
        }
      }
      for (const str of normalEfctList) {
        if (str === type.getId) {
          normalEfctTypeList.push(type);
        }
      }
      for (const str of notVeryEfctList) {
        if (str === type.getId) {
          notVeryEfctTypeList.push(type);
        }
      }
      for (const str of notVerySuperEfctList) {
        if (str === type.getId) {
          notVerySuperEfctTypeList.push(type);
        }
      }
      for (const str of nonEfctList) {
        if (str === type.getId) {
          nonEfctTypeList.push(type);
        }
      }
    }

    // data にバインディング
    this.verySuperEfctTypeList = verySuperEfctTypeList;
    this.superEfctTypeList = superEfctTypeList;
    this.normalEfctTypeList = normalEfctTypeList;
    this.notVeryEfctTypeList = notVeryEfctTypeList;
    this.notVerySuperEfctTypeList = notVerySuperEfctTypeList;
    this.nonEfctTypeList = nonEfctTypeList;
  }

  public clear(): void {
    this.verySuperEfctTypeList = [];
    this.superEfctTypeList = [];
    this.normalEfctTypeList = [];
    this.notVeryEfctTypeList = [];
    this.notVerySuperEfctTypeList = [];
    this.nonEfctTypeList = [];
    this.selectedValue1 = "none";
    this.selectedValue2 = "none";
  }
}

class Type {
  constructor(
    private id: string,
    private name: string,
    private superEfctList: Array<string>,
    private normalEfctList: Array<string>,
    private notVeryEfctList: Array<string>,
    private nonEfctList: Array<string>
  ) {
    this.id = id;
    this.name = name;
    this.superEfctList = superEfctList;
    this.normalEfctList = normalEfctList;
    this.notVeryEfctList = notVeryEfctList;
    this.nonEfctList = nonEfctList;
  }

  public get getId() {
    return this.id;
  }

  public get getName() {
    return this.name;
  }

  public get getSuperEfctList() {
    return this.superEfctList;
  }

  public get getNormalEfctList() {
    return this.normalEfctList;
  }

  public get getNotVeryEfctList() {
    return this.notVeryEfctList;
  }

  public get getNonEfctList() {
    return this.nonEfctList;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.row {
  margin-top: 10px;
  margin-bottom: 10px;
}

.type-style {
  color: white;
  width: 90px;
  text-align: center;
  border-radius: 5px;
  margin: 3px;
}

.normal {
  background-color: #b1b1b1;
}

.fire {
  background-color: #e4653f;
}

.water {
  background-color: #50abda;
}

.erectric {
  background-color: #e8c71a;
}

.grass {
  background-color: #9ac30e;
}

.ice {
  background-color: #53c4e5;
}

.fighting {
  background-color: #e99a3f;
}

.poison {
  background-color: #ba79c1;
}

.ground {
  background-color: #c8a841;
}

.flying {
  background-color: #5e9fe2;
}

.psychic {
  background-color: #e885b2;
}

.bug {
  background-color: #6cbe50;
}

.rock {
  background-color: #b08754;
}

.ghost {
  background-color: #756eb4;
}

.dragon {
  background-color: #6c81c8;
}

.dark {
  background-color: #5a3c1e;
}

.steel {
  background-color: #919191;
}

.fairy {
  background-color: #e06cbb;
}
</style>
