<template>
  <q-page class="flex flex-direction">
    <q-btn
      v-if="!btnGerar"
      flat
      class="full-width"
      label="Clique para gerar Currículo"
      push
      v-model="btnGerar"
      @click="gerar"
    />
    <q-card
      flat
      :bordered=false
      v-if="btnGerar"
    >
      <q-card-section align="between">
        <q-item v-if="btnGerar">
          <q-item-section avatar>
            <croppa
              :width="160"
              :height="160"
              canvas-color="#FFFFFF"
              placeholder="Escolher"
              :placeholder-font-size="14"
              placeholder-color="#000"
              :accept="'image/*'"
              :quality="1"
              initial-size="contain"
              :initial-image="Form.b64Foto"
              ref="b64Brasao"
            ></croppa>
          </q-item-section>
        </q-item>
        <q-card-actions class="q-pl-lg">
          <q-btn
            v-if="btnGerar"
            color="black"
            dense
            rounded
            outlined
            label="Gerar outro Currículo"
            push
            v-model="reset"
            @click="zerar"
          />
          <q-btn
            flat
            color="primary"
            @click="btnlerEscrever = !btnlerEscrever"
            icon="edit"
            label="Editar"
          />
        </q-card-actions>

      </q-card-section>
      <q-stepper
        v-model="step"
        vertical
        color="primary"
        animated
      >
        <q-step
          :name="1"
          title="Dados Pessoais"
          icon="edit"
          :done="step > 1"
        >
          <q-card-section>
            <q-separator />
            <q-card-section class="q-gutter-xs">
              <input
                v-model="Form.name"
                :readonly="btnlerEscrever"
                placeholder="Nome"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
              <input
                v-model="Form.dtNasc"
                :readonly="btnlerEscrever"
                placeholder="Data Nascimento"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <input
                v-model="Form.tel"
                :readonly="btnlerEscrever"
                placeholder="Telefone"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
              <input
                v-model="Form.endereco"
                :readonly="btnlerEscrever"
                placeholder="Endereço"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <input
                v-model="Form.RG"
                :readonly="btnlerEscrever"
                placeholder="RG"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
              <input
                v-model="Form.CPF"
                :readonly="btnlerEscrever"
                placeholder="CPF"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <input
                v-model="Form.email"
                :readonly="btnlerEscrever"
                placeholder="Email"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
              <input
                v-model="Form.age"
                :readonly="btnlerEscrever"
                placeholder="Idade"
                dense
                rounded
                outlined
                class="text-h6 q-mt-sm q-mb-xs"
              />
            </q-card-section>
          </q-card-section>
          <q-stepper-navigation>
            <q-btn
              @click="step = 2"
              color="primary"
              label="Continue"
            />
          </q-stepper-navigation>
        </q-step>
        <q-step
          :name="2"
          title="Formação Acadêmica"
          icon="edit"
          :done="step > 2"
        >
          <q-card-section>
            <q-separator />
            <q-card-section>
              <q-card-section class="q-gutter-xs q-pr-lg">
                <q-select
                  dense
                  rounded
                  outlined
                  v-model="Form.FormacaoAcademica"
                  :options="optionsAcad"
                  label="Escolha a opção"
                  emit-value
                  class="q-mt-sm q-mb-xs"
                >
                  <template v-slot:option="scope">
                    <q-item
                      v-bind="scope.itemProps"
                      v-on="scope.itemEvents"
                    >
                      <q-item-section>
                        <q-item-label v-html="scope.opt.label" />
                        <q-item-label caption>{{ scope.opt.description }}</q-item-label>
                      </q-item-section>
                    </q-item>
                  </template>
                </q-select>
                <input
                v-if="Form.FormacaoAcademica"
                v-model="Form.nomeDaEscola"
                :readonly="btnlerEscrever"
                placeholder="Nome da Escola"
                dense
                rounded
                outlined
                class="text-h6  q-mt-sm q-mb-xs"
              />
              </q-card-section>
            </q-card-section>
          </q-card-section>
           <q-stepper-navigation>
            <q-btn
              @click="step = 3"
              color="primary"
              label="Continue"
            />
          </q-stepper-navigation>
        </q-step>
        <q-step
          :name="3"
          title="Cursos Extracurriculares"
          icon="edit"
          :done="step > 3"
        >
          <q-card-section>
            <q-separator />
            <q-card-section>
              <q-card-section class="q-gutter-xs q-pr-lg">
                <q-select
                  dense
                  rounded
                  outlined
                  v-model="Form.FormacaoExtra"
                  :options="optionsExtra"
                  label="Escolha a opção"
                  emit-value
                  class="q-mt-sm q-mb-xs"
                >
                  <template v-slot:option="scope">
                    <q-item
                      v-bind="scope.itemProps"
                      v-on="scope.itemEvents"
                    >
                      <q-item-section>
                        <q-item-label v-html="scope.opt.label" />
                        <q-item-label caption>{{ scope.opt.description }}</q-item-label>
                      </q-item-section>
                    </q-item>
                  </template>
                </q-select>
                <input
                v-if="Form.FormacaoExtra"
                v-model="Form.nomeDaEscola"
                :readonly="btnlerEscrever"
                placeholder="Nome da Escola"
                dense
                rounded
                outlined
                class="text-h6  q-mt-sm q-mb-xs"
              />
                <input
                v-if="Form.nomeDaEscola.length > 7"
                v-model="Form.nomeDoCurso"
                :readonly="btnlerEscrever"
                placeholder="Informe o nome do Curso"
                dense
                rounded
                outlined
                class="text-h6  q-mt-sm q-mb-xs"
              />
              </q-card-section>
            </q-card-section>
          </q-card-section>
        </q-step>
        <q-stepper-navigation>
          <q-btn
            color="primary"
            label="Finalizar"
            @click="criarPDF"
          />
          <q-btn
            flat
            @click="step = 2"
            color="primary"
            label="Voltar"
            class="q-ml-sm"
          />
        </q-stepper-navigation>
      </q-stepper>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      optionsAcad: [
        {
          label: 'Fundamental',
          value: 'Fundamental Incompleto',
          description: 'Incompleto'
        },
        {
          label: 'Fundamental',
          value: 'Fundamental Completo',
          description: 'Completo'
        },
        {
          label: 'Médio',
          value: 'Médio Incompleto',
          description: 'Incompleto'
        },
        {
          label: 'Médio',
          value: 'Médio Completo',
          description: 'Completo'
        },
        {
          label: 'Superior',
          value: 'Superior Incompleto',
          description: 'Incompleto'
        },
        {
          label: 'Superior',
          value: 'Superior Completo',
          description: 'Completo'
        }
      ],
      optionsExtra: [
        {
          label: 'Técnico',
          value: 'Técnico'
        },
        {
          label: 'Línguas',
          value: 'Línguas Iniciante',
          description: 'Iniciante'
        },
        {
          label: 'Línguas',
          value: 'Línguas Intermediário',
          description: 'Intermediário'
        },
        {
          label: 'Línguas',
          value: 'Línguas Avançado',
          description: 'Avançado'
        }
      ],
      btnGerar: false,
      reset: '',
      step: 1,
      btnlerEscrever: true,
      Form: {
        name: '',
        dtNasc: '',
        tel: '',
        endereco: '',
        RG: '',
        CPF: '',
        email: '',
        age: '',
        FormacaoAcademica: '',
        nomeDaEscola: ''
      }
    }
  },
  methods: {
    gerar () {
      this.btnGerar = !this.btnGerar
    },
    zerar () {
      this.btnlerEscrever = !this.btnlerEscrever
      this.Form = {}
    },
    criarPDF () {
      alert('Criar PDF')
    }
  }
}
</script>
<style>
.croppa-container > canvas {
  background-color: white;
  border: 2px solid grey;
  border-radius: 130px;
  cursor: pointer;
}
</style>
