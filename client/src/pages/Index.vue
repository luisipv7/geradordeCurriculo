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
              ref="b64Foto"
            ></croppa>
          </q-item-section>
        </q-item>
        <q-card-actions class="q-pl-lg">
          <q-btn
            v-if="btnGerar"
            color="black"
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
              <q-input
                v-model="FormOne.name"
                :readonly="btnlerEscrever"
                placeholder="Nome"
                fit
                :error="$v.FormOne.name.$error "
                rounded
                outlined
              />
              <q-datetime-picker
                label="Data Nascimento"
                v-model="FormOne.dtNasc"
                :readonly="btnlerEscrever"
                :error="$v.FormOne.dtNasc.$error "
                dense
                rounded
                outlined
              ></q-datetime-picker>
              <!-- class="q-pr-lg" -->
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <q-input
                v-model="FormOne.tel"
                :readonly="btnlerEscrever"
                placeholder="Telefone"
                :error="$v.FormOne.tel.$error "
                fit
                type="tel"
                mask="(##) - ###.###.###"
                unmasked-value
                rounded
                outlined
              />
              <q-input
                v-model="FormOne.endereco"
                :readonly="btnlerEscrever"
                placeholder="Endereço"
                fit
                rounded
                outlined
                :error="$v.FormOne.endereco.$error "
              />
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <q-input
                v-model="FormOne.RG"
                :readonly="btnlerEscrever"
                label="RG"
                fit
                :error="$v.FormOne.RG.$error "
                rounded
                type="tel"
                outlined
              />
              <q-input
                v-model="FormOne.CPF"
                label="CPF's"
                :readonly="btnlerEscrever"
                :error="$v.FormOne.CPF.$error "
                fit
                mask="###.###.###-##"
                unmasked-value
                rounded
                outlined
              />
            </q-card-section>
            <q-card-section class="q-gutter-xs">
              <q-input
                v-model="FormOne.email"
                :readonly="btnlerEscrever"
                placeholder="Email"
                :error="$v.FormOne.email.$error "
                fit
                rounded
                outlined
              />
              <q-input
                v-model="FormOne.age"
                :readonly="btnlerEscrever"
                placeholder="Idade"
                :error="$v.FormOne.age.$error "
                fit
                mask="XXX.XXX.XXX-XX"
                rounded
                outlined
              />
            </q-card-section>
          </q-card-section>
          <q-stepper-navigation>
            <q-btn
              @click="nextStep(step)"
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
                  fit
                  rounded
                  outlined
                  v-model="FormTwo.FormacaoAcademica"
                  :options="optionsAcad"
                  label="Escolha a opção"
                  emit-value
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
                <q-input
                  v-if="FormTwo.FormacaoAcademica"
                  v-model="FormTwo.nomeDaEscola"
                  :readonly="btnlerEscrever"
                  placeholder="Nome da Escola"
                  fit
                  rounded
                  outlined
                  class=" q-mt-sm q-mb-xs"
                />
              </q-card-section>
            </q-card-section>
          </q-card-section>
          <q-stepper-navigation>
            <q-btn
              @click="nextStep(step)"
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
                <q-input
                  v-model="Experiencia.nomeDaEmpresa"
                  :readonly="btnlerEscrever"
                  placeholder="Nome da Empresa"
                  fit
                  rounded
                  outlined
                  class=" q-mt-sm q-mb-xs"
                />
                <q-input
                  v-model="Experiencia.cargo"
                  :readonly="btnlerEscrever"
                  placeholder="Cargo"
                  fit
                  rounded
                  outlined
                  class=" q-mt-sm q-mb-xs"
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
                  @click="addCampoExperiencia"
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
                  v-if="FormTwo.Experiencia.length > 0"
                >
                  <q-item
                    v-for="(info, index) in FormTwo.Experiencia"
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
                          fit
                          round
                          icon="delete"
                          @click="retirarCampoExperiencia(info)"
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
              @click="nextStep(step)"
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
                  fit
                  rounded
                  outlined
                  v-model="Cursos.FormacaoExtra"
                  :options="optionsExtra"
                  label="Escolha a opção"
                  emit-value
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
                <q-input
                  v-model="Cursos.nomeDaEscolaTecnica"
                  :readonly="btnlerEscrever"
                  placeholder="Nome da Escola"
                  fit
                  rounded
                  outlined
                  class=" q-mt-sm q-mb-xs"
                />
                <q-input
                  v-model="Cursos.nomeDoCurso"
                  :readonly="btnlerEscrever"
                  placeholder="Informe o nome do Curso"
                  fit
                  rounded
                  outlined
                  class=" q-mt-sm q-mb-xs"
                />
                <q-btn
                  color="primary"
                  @click="addCampoCurso"
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
                  v-if="FormTwo.Cursos.length > 0"
                >
                  <q-item
                    v-for="(infos, index) in FormTwo.Cursos"
                    :key="index"
                  >
                    <q-item-section top>
                      <q-item-label lines="1">
                        <span class="text-weight-medium">Nome da Escola:</span>
                        <span class="text-grey-8"> - {{ infos.nomeDaEscolaTecnica}}</span>
                      </q-item-label>
                      <q-item-label
                        caption
                        lines="1"
                      >
                        Grau do Curso: {{ infos.FormacaoExtra}}
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
                          fit
                          round
                          icon="delete"
                          @click="retirarCampoCurso(infos)"
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
import { required, minLength, email } from 'vuelidate/lib/validators'
import { isPhone, isCPF } from 'brazilian-values'
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
      Form: {},
      FormOne: {
        name: '',
        dtNasc: '',
        tel: '',
        endereco: '',
        RG: '',
        CPF: null,
        email: '',
        age: ''
      },
      FormTwo: {
        FormacaoAcademica: '',
        nomeDaEscola: '',
        Experiencia: [],
        Cursos: []
      }
    }
  },
  // TODO: Trocar para cada STEP um form diferente isso facilitara as validações
  validations: {
    FormOne: {
      name: {
        required,
        minLength: minLength(4)
      },
      dtNasc: {
        required
      },
      tel: {
        required,
        isPhone
      },
      RG: {
        required
      },
      CPF: {
        required,
        isCPF
      },
      email: {
        required,
        email
      },
      age: {
        required
      },
      endereco: {
        required
      },
      FormacaoAcademica: {
        required,
        minLength: minLength(7)
      },
      nomeDaEscola: {
        required,
        minLength: minLength(7)
      }
    }
  },
  methods: {
    nextStep (step) {
      switch (step) {
        case 1:
          this.$v.FormOne.name.$touch()
          this.$v.FormOne.dtNasc.$touch()
          this.$v.FormOne.tel.$touch()
          this.$v.FormOne.endereco.$touch()
          this.$v.FormOne.RG.$touch()
          this.$v.FormOne.CPF.$touch()
          this.$v.FormOne.email.$touch()
          this.$v.FormOne.age.$touch()
          break

        default:
          break
      }
      this.$v.FormOne.$error ? this.step-- : this.step++
    },
    gerar () {
      this.btnGerar = !this.btnGerar
    },
    zerar () {
      this.btnlerEscrever = !this.btnlerEscrever
      this.FormOne = {}
      this.FormTwo = {}
    },
    criarPDF () {
      this.Form.b64Foto = this.$refs.b64Foto ? this.$refs.b64Foto.generateDataUrl() : ''
      this.Form.push(this.FormOne)
      this.Form.push(this.FormTwo)
      alert(JSON.stringify(this.Form))
    },
    async addCampoExperiencia () {
      if (!this.Experiencia.dateFim) {
        return this.$q.notify({
          message: 'OPS!!!!',
          caption: 'Falta informação!',
          color: 'negative'
        })
      }
      const dataFim = dayjs(this.Experiencia.dateFim.substring(0, 10))
      const tempoYEAR = dataFim.diff(this.Experiencia.dateIni.substring(0, 10), 'year')
      const tempoMONTH = dataFim.diff(this.Experiencia.dateIni.substring(0, 10), 'month')

      if (tempoYEAR > 0) {
        this.Experiencia.tempo = `${tempoYEAR} Ano(s)`
      } else {
        this.Experiencia.tempo = tempoMONTH > 1 ? `${tempoMONTH} Meses` : `${tempoMONTH} Mês`
      }
      await this.Form.Experiencia.push(this.Experiencia)
      this.Experiencia = {
        nomeDaEmpresa: '',
        cargo: '',
        dateIni: null,
        dateFim: null,
        tempo: ''
      }
      return this.$q.notify({
        message: 'Cadastro',
        caption: 'Cadastrado com sucesso!',
        color: 'positive'
      })
    },
    addCampoCurso () {
      if (!this.Cursos.nomeDaEscolaTecnica.length > 0) {
        return this.$q.notify({
          message: 'OPS!!!!',
          caption: 'Falta informação!',
          color: 'negative'
        })
      }
      this.Form.Cursos.push(this.Cursos)
      this.Cursos = {
        nomeDaEscolaTecnica: '',
        FormacaoExtra: ''
      }
      return this.$q.notify({
        message: 'Cadastro',
        caption: 'Cadastrado com sucesso!',
        color: 'positive'
      })
    },
    retirarCampoExperiencia (info) {
      this.Form.Experiencia = this.Form.Experiencia.filter(R => R.nomeDaEmpresa !== info.nomeDaEmpresa)
    },
    retirarCampoCurso (info) {
      this.Form.Cursos = this.Form.Cursos.filter(R => R.nomeDaEscolaTecnica !== info.nomeDaEscolaTecnica)
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
