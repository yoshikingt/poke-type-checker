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
                <div v-for="str of verySuperEfctList" v-bind:key="str">
                  {{ str }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>2倍弱点</b-th>
              <b-td>
                <div v-for="str of superEfctList" v-bind:key="str">
                  {{ str }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>1/2耐性</b-th>
              <b-td>
                <div v-for="str of notVeryEfctList" v-bind:key="str">
                  {{ str }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>1/4耐性</b-th>
              <b-td>
                <div v-for="str of notVerySuperEfctList" v-bind:key="str">
                  {{ str }}
                </div>
              </b-td>
            </b-tr>
            <b-tr>
              <b-th>こうかなし</b-th>
              <b-td>
                <div v-for="str of nonEfctList" v-bind:key="str">
                  {{ str }}
                </div>
              </b-td>
            </b-tr>
          </b-tbody>
        </b-table-simple>
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Main extends Vue {
  /** data */
  private typeList: ReadonlyArray<Type> = [
    new Type("none", "なし", [], [], [], []),
    new Type(
      "normal",
      "ノーマル",
      ["かくとう"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "どく",
        "じめん",
        "ひこう",
        "エスパー",
        "むし",
        "いわ",
        "ドラゴン",
        "あく",
        "はがね",
        "フェアリー"
      ],
      [""],
      ["ゴースト"]
    ),
    new Type(
      "fire",
      "ほのお",
      ["みず", "じめん", "いわ"],
      [
        "ノーマル",
        "でんき",
        "かくとう",
        "どく",
        "ひこう",
        "エスパー",
        "ゴースト",
        "ドラゴン",
        "あく"
      ],
      ["ほのお", "くさ", "こおり", "むし", "はがね", "フェアリー"],
      []
    ),
    new Type(
      "water",
      "みず",
      ["でんき", "くさ"],
      [
        "ノーマル",
        "かくとう",
        "どく",
        "じめん",
        "ひこう",
        "エスパー",
        "むし",
        "いわ",
        "ゴースト",
        "ドラゴン",
        "あく",
        "フェアリー"
      ],
      ["ほのお", "みず", "こおり", "はがね"],
      []
    ),
    new Type(
      "erectric",
      "でんき",
      ["じめん"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "くさ",
        "こおり",
        "かくとう",
        "どく",
        "エスパー",
        "むし",
        "いわ",
        "ゴースト",
        "ドラゴン",
        "あく",
        "フェアリー"
      ],
      ["でんき", "ひこう", "はがね"],
      []
    ),
    new Type(
      "grass",
      "くさ",
      ["ほのお", "こおり", "どく", "ひこう", "むし"],
      [
        "ノーマル",
        "かくとう",
        "エスパー",
        "いわ",
        "ゴースト",
        "ドラゴン",
        "あく",
        "はがね",
        "フェアリー"
      ],
      ["みず", "でんき", "くさ", "じめん"],
      []
    ),
    new Type(
      "ice",
      "こおり",
      ["ほのお", "かくとう", "いわ", "はがね"],
      [
        "ノーマル",
        "みず",
        "でんき",
        "くさ",
        "どく",
        "じめん",
        "ひこう",
        "エスパー",
        "むし",
        "ゴースト",
        "ドラゴン",
        "あく",
        "フェアリー"
      ],
      ["こおり"],
      []
    ),
    new Type(
      "fighting",
      "かくとう",
      ["ひこう", "エスパー", "フェアリー"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "かくとう",
        "どく",
        "じめん",
        "ゴースト",
        "ドラゴン",
        "はがね"
      ],
      ["むし", "いわ", "あく"],
      []
    ),
    new Type(
      "poison",
      "どく",
      ["じめん", "エスパー"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "こおり",
        "ひこう",
        "いわ",
        "ゴースト",
        "ドラゴン",
        "あく",
        "はがね"
      ],
      ["くさ", "かくとう", "どく", "むし", "フェアリー"],
      []
    ),
    new Type(
      "ground",
      "じめん",
      ["みず", "くさ", "こおり"],
      [
        "ノーマル",
        "ほのお",
        "かくとう",
        "じめん",
        "ひこう",
        "エスパー",
        "むし",
        "ゴースト",
        "ドラゴン",
        "あく",
        "はがね",
        "フェアリー"
      ],
      ["どく", "いわ"],
      ["でんき"]
    ),
    new Type(
      "flying",
      "ひこう",
      ["でんき", "こおり", "いわ"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "どく",
        "ひこう",
        "エスパー",
        "ゴースト",
        "ドラゴン",
        "あく",
        "はがね",
        "フェアリー"
      ],
      ["くさ", "かくとう", "むし"],
      ["じめん"]
    ),
    new Type(
      "psychic",
      "エスパー",
      ["むし", "ゴースト", "あく"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "どく",
        "じめん",
        "ひこう",
        "いわ",
        "ドラゴン",
        "はがね",
        "フェアリー"
      ],
      ["かくとう", "エスパー"],
      []
    ),
    new Type(
      "bug",
      "むし",
      ["ほのお", "ひこう", "いわ"],
      [
        "ノーマル",
        "みず",
        "でんき",
        "こおり",
        "どく",
        "エスパー",
        "むし",
        "ゴースト",
        "ドラゴン",
        "あく",
        "はがね",
        "フェアリー"
      ],
      ["くさ", "かくとう", "じめん"],
      []
    ),
    new Type(
      "rock",
      "いわ",
      ["みず", "くさ", "かくとう", "じめん", "はがね"],
      [
        "でんき",
        "こおり",
        "エスパー",
        "むし",
        "いわ",
        "ゴースト",
        "ドラゴン",
        "あく",
        "フェアリー"
      ],
      ["ノーマル", "ほのお", "どく", "ひこう"],
      []
    ),
    new Type(
      "ghost",
      "ゴースト",
      ["ゴースト", "あく"],
      [
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "じめん",
        "ひこう",
        "エスパー",
        "いわ",
        "ドラゴン",
        "はがね",
        "フェアリー"
      ],
      ["どく", "むし"],
      ["ノーマル", "かくとう"]
    ),
    new Type(
      "dragon",
      "ドラゴン",
      ["こおり", "ドラゴン", "フェアリー"],
      [
        "ノーマル",
        "かくとう",
        "どく",
        "じめん",
        "ひこう",
        "エスパー",
        "むし",
        "いわ",
        "ゴースト",
        "あく",
        "はがね"
      ],
      ["ほのお", "みず", "でんき", "くさ"],
      []
    ),
    new Type(
      "dark",
      "あく",
      ["かくとう", "むし", "フェアリー"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "どく",
        "じめん",
        "ひこう",
        "いわ",
        "ドラゴン",
        "はがね"
      ],
      ["ゴースト", "あく"],
      ["エスパー"]
    ),
    new Type(
      "steel",
      "はがね",
      ["ほのお", "かくとう", "じめん"],
      ["みず", "でんき", "ゴースト", "あく"],
      [
        "ノーマル",
        "くさ",
        "こおり",
        "ひこう",
        "エスパー",
        "むし",
        "いわ",
        "ドラゴン",
        "はがね",
        "フェアリー"
      ],
      ["どく"]
    ),
    new Type(
      "fairy",
      "フェアリー",
      ["どく", "はがね"],
      [
        "ノーマル",
        "ほのお",
        "みず",
        "でんき",
        "くさ",
        "こおり",
        "じめん",
        "ひこう",
        "エスパー",
        "いわ",
        "ゴースト",
        "フェアリー"
      ],
      ["かくとう", "むし", "あく"],
      ["ドラゴン"]
    )
  ];
  private verySuperEfctList: Array<string> = [];
  private superEfctList: Array<string> = [];
  private normalEfctList: Array<string> = [];
  private notVeryEfctList: Array<string> = [];
  private notVerySuperEfctList: Array<string> = [];
  private nonEfctList: Array<string> = [];
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

    this.verySuperEfctList = verySuperEfctList;
    this.superEfctList = superEfctList;
    this.normalEfctList = normalEfctList;
    this.notVeryEfctList = notVeryEfctList;
    this.notVerySuperEfctList = notVerySuperEfctList;
    this.nonEfctList = nonEfctList;
  }

  public clear(): void {
    this.verySuperEfctList = [];
    this.superEfctList = [];
    this.normalEfctList = [];
    this.notVeryEfctList = [];
    this.notVerySuperEfctList = [];
    this.nonEfctList = [];
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
</style>
