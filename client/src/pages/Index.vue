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
      class="q-pr-lg"
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
          title="Experiência Profissional"
          icon="edit"
          :done="step > 3"
        >
          <q-card-section>
            <q-separator />
            <q-card-section>
              <q-card-section class="q-gutter-xs q-pr-lg">
                <input
                  v-model="Experiencia.nomeDaEmpresa"
                  :readonly="btnlerEscrever"
                  placeholder="Nome da Empresa"
                  dense
                  rounded
                  outlined
                  class="text-h6  q-mt-sm q-mb-xs"
                />
                <input
                  v-model="Experiencia.cargo"
                  :readonly="btnlerEscrever"
                  placeholder="Cargo"
                  dense
                  rounded
                  outlined
                  class="text-h6  q-mt-sm q-mb-xs"
                />
                <q-datetime-picker
                  label="Data de Inicio"
                  v-model="Experiencia.dateIni"
                  outlined
                ></q-datetime-picker>
                <q-datetime-picker
                  label="Data de Saída"
                  v-model="Experiencia.dateFim"
                  outlined
                ></q-datetime-picker>
                <q-btn
                  color="primary"
                  @click="addCampo(Experiencia, null)"
                >
                  <q-icon
                    left
                    round
                    size="3em"
                    name="add"
                  />
                  <div>Adicionar Novo</div>
                </q-btn>
                <q-list
                  bordered
                  separator
                  v-if="Form.Experiencia.length > 0"
                >
                  <q-item
                    v-for="(info, index) in Form.Experiencia"
                    :key="index"
                  >
                    <q-item-section top>
                      <q-item-label lines="1">
                        <span class="text-weight-medium">Nome da Empresa:</span>
                        <span class="text-grey-8"> - {{ info.nomeDaEmpresa}}</span>
                      </q-item-label>
                      <q-item-label
                        caption
                        lines="1"
                      >
                        Cargo: {{ info.cargo}}
                      </q-item-label>
                      <q-item-label
                        caption
                        lines="1"
                      >
                        <span>Tempo de Empresa: {{ info.tempo }} </span>
                      </q-item-label>
                    </q-item-section>

                    <q-item-section
                      top
                      side
                    >
                      <div class="text-grey-8 q-gutter-xs">
                        <q-btn
                          size="12px"
                          flat
                          dense
                          round
                          icon="delete"
                          @click="retirarCampo(info)"
                        />
                      </div>
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-card-section>
            </q-card-section>
          </q-card-section>
          <q-stepper-navigation>
            <q-btn
              @click="step = 4"
              color="primary"
              label="Continue"
            />
          </q-stepper-navigation>
        </q-step>
        <q-step
          :name="4"
          title="Cursos Extracurriculares"
          icon="edit"
          :done="step > 4"
        >
          <q-card-section>
            <q-separator />
            <q-card-section>
              <q-card-section class="q-gutter-xs q-pr-lg">
                <q-select
                  dense
                  rounded
                  outlined
                  v-model="Cursos.FormacaoExtra"
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
                  v-if="Cursos.FormacaoExtra"
                  v-model="Cursos.nomeDaEscolaTecnica"
                  :readonly="btnlerEscrever"
                  placeholder="Nome da Escola"
                  dense
                  rounded
                  outlined
                  class="text-h6  q-mt-sm q-mb-xs"
                />
                <input
                  v-if="Cursos.nomeDaEscolaTecnica.length > 7"
                  v-model="Cursos.nomeDoCurso"
                  :readonly="btnlerEscrever"
                  placeholder="Informe o nome do Curso"
                  dense
                  rounded
                  outlined
                  class="text-h6  q-mt-sm q-mb-xs"
                />
                <q-btn
                  color="primary"
                  @click="addCampo(null, Cursos)"
                >
                  <q-icon
                    left
                    round
                    size="3em"
                    name="add"
                  />
                  <div>Adicionar Novo</div>
                </q-btn>
                <q-list
                  bordered
                  separator
                  v-if="Form.Experiencia.length > 0"
                >
                  <q-item
                    v-for="(info, index) in Form.Cursos"
                    :key="index"
                  >
                    <q-item-section top>
                      <q-item-label lines="1">
                        <span class="text-weight-medium">Nome da Escola:</span>
                        <span class="text-grey-8"> - {{ info.nomeDaEscolaTecnica}}</span>
                      </q-item-label>
                      <q-item-label
                        caption
                        lines="1"
                      >
                        Grau do Curso: {{ info.FormacaoExtra}}
                      </q-item-label>
                    </q-item-section>

                    <q-item-section
                      top
                      side
                    >
                      <div class="text-grey-8 q-gutter-xs">
                        <q-btn
                          size="12px"
                          flat
                          dense
                          round
                          icon="delete"
                          @click="retirarCampo(info)"
                        />
                      </div>
                    </q-item-section>
                  </q-item>
                </q-list>
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
            @click="step = step - 1"
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
import dayjs from 'dayjs'
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
      Experiencia: {
        nomeDaEmpresa: '',
        cargo: '',
        dateIni: null,
        dateFim: null,
        tempo: ''
      },
      Cursos: {
        nomeDaEscolaTecnica: '',
        FormacaoExtra: ''
      },
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
        nomeDaEscola: '',
        Experiencia: [],
        Cursos: []
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
      alert(JSON.stringify(this.Form.Experiencia))
    },
    addCampo (Exp, Curso) {
      if (Exp && Exp !== this.Experiencia) {
        console.log(Exp)
        const dataFim = dayjs(Exp.dateFim.substring(0, 10))
        const tempoYEAR = dataFim.diff(Exp.dateIni.substring(0, 10), 'year')
        const tempoMONTH = dataFim.diff(Exp.dateIni.substring(0, 10), 'month')

        if (tempoYEAR > 0) {
          Exp.tempo = `${tempoYEAR} Ano(s)`
        } else {
          Exp.tempo = tempoMONTH > 1 ? `${tempoMONTH} Meses` : `${tempoMONTH} Mês`
        }
        this.Form.Experiencia.push(Exp)
        this.Experiencia = {
          nomeDaEmpresa: '',
          cargo: '',
          dateIni: null,
          dateFim: null,
          tempo: ''
        }
      } else {
        if (Curso !== this.Cursos) {
          this.Form.Cursos.push(Curso)
          this.Cursos = {
            nomeDaEscolaTecnica: '',
            FormacaoExtra: ''
          }
        }
      }
      return this.$q.notify({
        message: 'OPS!!!',
        caption: 'Falta Informações',
        color: 'negative'
      })
    },
    retirarCampo (info) {
      console.log(info)
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
