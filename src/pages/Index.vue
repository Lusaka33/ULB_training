<template>
	<q-page class="flex flex-center">
		<div class="fixed-bottom-right bg-deep-purple-6 text-center"
				 style="width: 50px; border-radius: 10%; z-index: 999">
				{{ minutes }} : {{ seconds }}
		</div>
		<transition enter-active-class="animated fadeIn"
								leave-active-class="animated fadeOut"
								mode="out-in"
								:duration="300">
			<div class="test"
					 :key="1"
					 v-if="test">
				<q-stepper
					animated
					v-model="step"
					ref="stepper"
					vertical
				>
					<q-step
						v-for="(item, i) in quiz"
						:key="i"
						:title="item.title"
						:name="i + 1"
						:done="item.result === true"
						:error="item.result === false"
						done-color="green"
						active-color="deep-orange"
					>
						<div class="q-gutter-sm
							column
							rounded-borders
							styling__radio">
							<q-radio
								color="deep-orange"
								v-for="(question, j) in item.questions"
								:key="j"
								v-model="item.answer"
								:val="j + 1"
								:label="question"/>
						</div>
						<q-stepper-navigation>
							<q-btn
								v-if="step < quiz.length"
								@click="checkAnswer(i)"
								color="deep-orange">
								Next
							</q-btn>
							<q-btn
								:loading="loading"
								v-if="step >= quiz.length"
								@click="checkAnswer(i); showResult()"
								color="green">
								Get result
							</q-btn>
						</q-stepper-navigation>
					</q-step>
				</q-stepper>
			</div>
			<div
				class="result"
				:key="2"
				v-if="!test"
			>
				<div v-if="resultArr.length === 0">
					<p class="text-h3 bg-green">Look at you! You did no mistake!</p>
				</div>
				<div v-else>
					<p class="text-h4 text-center">You did mistakes in those questions</p>
					<q-card
						v-for="(result, i) in this.resultArr"
						class="q-mb-md"
						:key="i">
						<q-card-section
							class="text-center"
							style="background: lightgoldenrodyellow">
							{{ result.title }}
						</q-card-section>
						<q-card-section style="background: lightcoral">
							<p class="flex justify-center">Your answer:</p>
							<p>{{ !result.answer ? "You didn't select answer" : result.questions[result.answer - 1] }}</p>
						</q-card-section>
						<q-card-section style="background: lightgreen">
							<p class="flex justify-center">Right answer:</p>
							<p>{{ result.questions[result.rightAnswer - 1] }}</p>
						</q-card-section>
					</q-card>
				</div>
			</div>
		</transition>
	</q-page>
</template>

<script>
export default {
	name: 'PageIndex',
	data(){
		return {
			interval: null,
			minutes: 60,
			seconds: 0,
			loading: false,
			step: 1,
			test: true,
			resultArr: [],
			quiz: [
				{
					title: "Patiente de 54 ans, sans aucun antécédent personnel ou familial hormis une consommation tabagique. Céphalée brutale, qu'elle décrit comme la pire de sa vie. Examen clinique: raideur de nuque, photophobie, hémiparésie droite. Quel est le 1er examen à réaliser en urgence ?",
					questions: [
						"IRM cérébrale",
						"Ponction lombaire",
						"Angiographie cérébrale",
						"CT scanner"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quel est le grade clinique WFNS de la patiente ?",
					questions: [
						"WFNS 1",
						"WFNS 2",
						"WFNS 3",
						"WFNS 4"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un anévrysme rompu est diagnostiqué. On sait que :",
					questions: [
						"Les anévrysmes les plus fréquents sont situés en circulation postérieure",
						"Les anévrysmes de l’artère carotide interne sont très rares",
						"Les anévrysmes de l’artère communicante antérieure sont les plus fréquents",
						"Les anévrysmes sont le plus souvent situés à distance du Polygone de Willis"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La patiente est traitée et sa sortie est prévue. Vous lui recommandez :",
					questions: [
						"La prise de statines",
						"L’arrêt du tabac et un contrôle de sa tension artérielle",
						"L’arrêt de toute activité sportive",
						"Un dépistage pour ses enfants"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patient de 70 ans, se présentant avec une difficulté à la marche et une diminution des mouvements fins des mains. L'examen neurologique démontre: un cutané plantaire en extension, des réflexes vifs, un Hoffmann bilatéral. Quelle est votre hypothèse diagnostique ?",
					questions: [
						"canal cervical étroit",
						"myélopathie cervicarthrosique",
						"tumeur intra-médullaire",
						"a et b"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Patient de 70 ans, se présentant avec une difficulté à la marche et une diminution des mouvements fins des mains. L'examen neurologique démontre: un cutané plantaire en extension, des réflexes vifs, un Hoffmann bilatéral. Comment confirmez-vous votre hypothèse ?",
					questions: [
						"par une IRM",
						"par une RX",
						"par un CT avec injection",
						"a et c"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Patient de 70 ans, se présentant avec une difficulté à la marche et une diminution des mouvements fins des mains. L'examen neurologique démontre: un cutané plantaire en extension, des réflexes vifs, un Hoffmann bilatéral. Quel type de traitement proposeriez-vous ?",
					questions: [
						"analgésique uniquement",
						"chirurgical",
						"par péridurale",
						"a et c"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel examen permet de voir l'intégrité de la moelle épinière ?",
					questions: [
						"une EMG",
						"un CT-Scan",
						"une IRM",
						"des PES"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un petit enfant de 10 mois présente un torticolis traité à plusieurs reprises par de l'ostéopathie. Son examen neurologique est normal mais l'enfant est moins vif, d'après les parents. Quelle est votre attitude ?",
					questions: [
						"rien",
						"continuer l'ostéopathie",
						"demander une imagerie",
						"aucune des propositions"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un petit enfant de 10 mois présente un torticolis traité à plusieurs reprises par de l'ostéopathie. Son examen neurologique est normal mais l'enfant est moins vif, d'après les parents. Comment expliquer ce torticolis ?",
					questions: [
						"par une position basse de l'amygdale cérébelleuse",
						"par un engagement cérébelleux",
						"par un spasme du sterno-cléido-mastoïdien",
						"toutes les propositions sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Patient de 66 ans, sans antécédent particulier. Aphasie et hémiplégie gauche brutale. Arrivée aux urgences 2 heures après l’ictus. Mise au point par CT-Scanner montre l’absence d’hémorragie cérébrale, une occlusion sylvienne proximale par un caillot, et une zone de pénombre importante au CT-Scanner de perfusion. Quel(s) traitement(s) doit(doivent) être proposé(s) au plus vite?",
					questions: [
						"Fibrinolyse IV",
						"Thrombectomie intracrânienne (retrait caillot par voie endovasculaire)",
						"Aucun, surveillance à la Stroke Unit",
						"Fibrinolyse IV + thrombectomie"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "La stéréotaxie:",
					questions: [
						"est un traitement neurochirurgical de la surdité",
						"est une technique chirurgicale consistant à utiliser des coordonnées précises de l'espace tridimensionnel pour guider des gestes thérapeutiques ou diagnostiques neurochirurgicaux",
						"est utilisée dans le traitement des hernies discales",
						"n'est pas utilisée pour des interventions chirurgicales au niveau du cerveau"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "La névralgie du trijumeau:",
					questions: [
						"se présente toujours sous la forme de douleurs continues peu intenses du visage",
						"ne peut être traitée que par médicaments",
						"est toujours associée à une sclérose en plaques",
						"peut faire l'objet de plusieurs techniques de traitement chirurgical différentes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "La radiochirurgie par Gamma Knife :",
					questions: [
						"se base sur un principe de concentration de rayons ionisants",
						"peut être utilisée pour le traitement du canal lombaire étroit",
						"est parfois utile dans le traitement des traumatisés crâniens sévères en phase aiguë",
						"ne peut jamais être utilisée pour le traitement de tumeurs cérébrales bénignes"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "La chirurgie de la douleur :",
					questions: [
						"comprend des techniques réalisées seulement au niveau cérébral",
						"comprend des techniques réalisées seulement au niveau de la moëlle épinière",
						"comprend des techniques réalisées au niveau cérébral et au niveau de la moëlle épinière",
						"ne s'adresse jamais aux douleurs intenses",
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Une hydrocéphalie chez un enfant de 3 mois donne comme signes :",
					questions: [
						"une augmentation du PC",
						"une fontanelle sous tensio",
						"des yeux en coucher de soleil",
						"toutes les propositions sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Des troubles visuels, un manque de libido et des maux de tête doivent vous faire suspecter :",
					questions: [
						"une lésion épiphysair",
						"une lésion hypophysaire",
						"une lésion hypothalamique",
						"tout est correct"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel type de craniosténose doit absolument se faire traiter chirurgicalement ?",
					questions: [
						"Scaphocéphalie",
						"Plagiocéphalie postérieure",
						"Trigonocéphalie",
						"Anencéphalie"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Les symptômes des canaux lombaires étroits sont diminués par",
					questions: [
						"La station debout",
						"La marche",
						"La bicyclette",
						"Le terrain en pente"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un engagement temporal est :",
					questions: [
						"Une descente de l’uncus dans le foramen de Pacchioni",
						"Un signe d’hypotension intracrânienne",
						"Une compression du bulbe rachidien",
						"A l’origine du torticolis"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. À quoi pensez-vous ?",
					questions: [
						"à une gastro-entérite",
						"à une infection viralel",
						"à une tumeur cérébrale",
						"à une hémorragie cérébrale"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quelle mise au point effectuez-vous ?",
					questions: [
						"un EEG",
						"un fond d’œil",
						"une imagerie médicale",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quel traitement proposez-vous ?",
					questions: [
						"une chimiothérapie",
						"une chirurgie",
						"une radiothérapie",
						"aucune réponse n’est correcte"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quelle est la tumeur la plus fréquente chez l’enfant ?",
					questions: [
						"un astrocytome pilocytique",
						"un glioblastome",
						"une tumeur embryonnaire",
						"a et c sont correctes"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Dans les médulloblastomes chez les enfants, le pronostic dépend la plus fréquente chez l’enfant ?",
					questions: [
						"de la taille de la résection",
						"des anomalies génétiques",
						"de l’âge",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent.Quelle est son échelle de Glasgow ?",
					questions: [
						"4",
						"4T",
						"5T",
						"5"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. Quelle prise en charge préconisez-vous ?",
					questions: [
						"la mise en place d’une PIC",
						"une ponction lombaire",
						"l’administration de mannitol",
						"une surveillance"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. La ponction lombaire ne peut se faire que ?",
					questions: [
						"lorsqu’une imagerie a exclu un processus expansif",
						"au niveau L4-L5",
						"au niveau L5-S1",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. La mesure de pression intracrânienne est indiquée dans les traumatismes crâniens lorsque :",
					questions: [
						"on note des signes d’hypertension intracrânienne",
						"le Glasgow est inférieur à 7",
						"le scanner est anormal",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. Dans les traumatismes crâniens, l’hypercapnie peut entrainer :",
					questions: [
						"une vasodilatation",
						"l’augmentation de la PAO2",
						"une diminution du diamètre des artères",
						"toutes les réponses sont correcte"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. Les ondes de la mesure de PIC fluctuent en fonction :",
					questions: [
						"de la respiration",
						"du rythme cardiaque",
						"de la toux",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. La pression est supérieure à 20 mm Hg. Que proposez-vous?",
					questions: [
						"le drainage du liquide céphalorachidien",
						"le contrôle de la tension artérielle",
						"5de mettre la tête du patient à 30°",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. Quelle(s) est (sont) la (les) complication(s) possible(s) chez ces patients :",
					questions: [
						"un vasospasme",
						"une hydrocéphalie",
						"une ventriculite (infection des ventricules)",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. L’hémorragie sous-arachnoïdienne peut avoir comme origine la plus probable :",
					questions: [
						"un traumatisme crânien",
						"la rupture d’un anévrisme",
						"la rupture d’une malformation artério-veineuse",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Quel est le 1er examen à réaliser ?",
					questions: [
						"une IRM cérébrale",
						"un CT-Scanner",
						"une ponction lombaire",
						"une angiographie cérébrale"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Quel est le grade clinique WFNS de la patiente :",
					questions: [
						"WFNS 1",
						"WFNS 2",
						"WFNS 3",
						"WFNS 4"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Un anévrisme est diagnostiqué. On sait que :",
					questions: [
						"les anévrismes les plus fréquents sont situés en circulation postérieure",
						"les anévrismes de l’artère carotide interne sont très rares",
						"les anévrismes sont le plus souvent situés à distance du polygone de Willis",
						"les anévrismes de l’artère communicante antérieure sont les plus fréquents"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "La myélinisation",
					questions: [
						"est un phénomène dynamique qui ne se fait pas au même moment pour toutes les structures du cerveau",
						"commence d’abord dans les lobes temporaux",
						"s’arrête à l’adolescence",
						"commence d’abord dans les lobes occipitaux"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quelle est la présentation clinique d’un enfant âgé de 2 ans après un AVC sylvien congénital gauche",
					questions: [
						"Aphasie",
						"Hémiplégie droite + aphasie",
						"Hémiplégie droite",
						"Diplégie spastique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La grande prématurité",
					questions: [
						"s’accompagne de troubles cognitifs et comportementaux dans 5 à 10% des cas",
						"s’accompagne de troubles cognitifs et comportementaux dans 25 à 50% des c",
						"s’accompagne dans plus de 15% des cas d’une infirmité motrice cérébrale de type diplégie spastique",
						"concerne les enfants nés entre 32 et 37 semaines d’âge gestationnel"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patient de 70 ans, se présentant avec une diffculté à la marche et une diminution des mouvements fins des mains. L'examen neurologique démontre un cutané plantaire en extension, des réflexes vifs, un Hoffman bilatéral. Quelle est votre hypothèse diagnostique ?",
					questions: [
						"canal cervical étroit",
						"myélopathie cervicarthrosique",
						"tumeur intra-médullaire",
						"a et b"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Comment confirmez-vous votre hypothèse?",
					questions: [
						"par une IRM",
						"par une RX",
						"par un CT avec injection",
						"aetc"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quel type de traitement proposeriez-vous?",
					questions: [
						"analgésique uniquement",
						"chirurgical",
						"par péridural",
						"aetc"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel examen permet de voir l'intégrité de la moelle épinière?",
					questions: [
						"une EMG",
						"un CT-Scan",
						"une IRM",
						"des PES"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Suite à un accident de roulage sans ceinture de sécurité, un patient arrive aux urgences après réanimation avec un GCS (échelle de Glasgow) de 5/15, une mydriase droite xe et une hémiparésie droite. Patient inconscient directement. Quelle prise en charge que proposeriez-vous ?",
					questions: [
						"manœuvre 'ABC'",
						"examen physique complet",
						"examen neurologique complet",
						"Réalisation d'une imagerie"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Comment expliquez-vous la mydriase et l’hémiparésie ?",
					questions: [
						"engagement uncus droit",
						"engagement gyrus frontal droit",
						"engagement des amygdales pharyngiennes",
						"aucune réponse"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Comment expliquer l'hémiparésie ipsilatérale",
					questions: [
						"lésion du faisceau pyramidal par lésion médullaire complète",
						"lésion du faisceau pyramidal par l'uncus droit",
						"lésion du faisceau pyramidal par la tente du cervelet",
						"lésion du faisceau pyramidal par l'uncus gauche"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "L'imagerie démontre un hématome de type biconvexe et spontanément hyperdense. De quoi s'agit-il?",
					questions: [
						"hématome sous-dural aigu",
						"hématome sous-dural chronique",
						"hématome extra-dural",
						"hématome intra-parenchymateux"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Son pronostic dépend de :",
					questions: [
						"la vitesse de prise en charge",
						"l'âge du patient",
						"la valeur de sa PIC",
						"aetb"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Combien de Belges font une rupture d'anévrisme par an :",
					questions: [
						"110",
						"1100",
						"11000",
						"110000"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Facteurs de risque pour développer un anévrisme : trouvez l'intrus",
					questions: [
						"HTA",
						"tabac",
						"diabète",
						"sexe féminin"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle est la localisation la plus fréquente des anévrismes :",
					questions: [
						"l’artère communicante postérieure",
						"l’artère communicante antérieure",
						"l’artère sylvienne",
						"le tronc basilaire"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Une patiente fait une première crise d'épilepsie dans un ciel serein. Quelle est votre attitude?",
					questions: [
						"vous demandez une imagerie avec et sans contraste",
						"vous demandez une imagerie sans contraste",
						"vous demandez une imagerie avec et sans contraste et un anti-épileptique",
						"vous demandez une imagerie sans contraste et un anti- épileptique"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "L'imagerie démontre une masse extra-axiale au niveau de la convexité. A quoi pensez-vous ?",
					questions: [
						"à un glioblastome",
						"à un astrocytome",
						"à un méningiome",
						"à une métastase"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle prise en charge préconisez-vous ?",
					questions: [
						"une chirurgie",
						"une chirurgie et une radiothérapie",
						"une chirurgie et une chimiothérapie",
						"une chirurgie, une radiothérapie et une chimiothérapie"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quelle est la tumeur maligne primitive la plus fréquente ?",
					questions: [
						"la métastase",
						"le glioblastome",
						"le méningiome",
						"l’épendymome"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un petit enfant de 10 mois présente un torticolis traité à plusieurs reprises par de l'ostéopathie. Il présente un examen neurologique normal mais est moins vif, d'après les parents. Quelle est votre attitude?",
					questions: [
						"rien",
						"continuer l'ostéopathie",
						"demander une imagerie",
						"aucune des propositions"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un petit enfant de 10 mois présente un torticolis traité à plusieurs reprises par de l'ostéopathie. Il présente un examen neurologique normal mais est moins vif, d'après les parents. Comment expliquer ce torticolis?",
					questions: [
						"par une position basse de l'amygdale cérébelleuse",
						"par un engagement cérébelleux",
						"par un spasme du sternocléidomastoïdien",
						"toutes les propositions sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Une hydrocéphalie chez un enfant de 3 mois donne comme signes",
					questions: [
						"une augmentation du PC",
						"une fontanelle sous tension",
						"des yeux en coucher de soleil",
						"toutes les propositions sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Des troubles visuels, un manque de libido et des maux de tête doivent vous faire suspecter :",
					questions: [
						"une lésion épiphysaire",
						"une lésion hypophysaire",
						"une lésion hypothalamique",
						"tout est correct"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Patient de 35 se présente avec une névrite cervico-brachial droite à prédominance nocturne. La douleur irradie jusqu’au pouce, il n’y a pas de déficit moteur. Les Mouvements du cou n’entrainent aucune douleur. Un scanner sans injection est normal. Quelle est votre hypothèse diagnostique ?",
					questions: [
						"hernie dicale (mouvement rachis sans douleur exclut atteinte primaire rachidienne : Hernie, arthrose)",
						"Neurinome",
						"Tumeur intramédullaire",
						"AetB",
						"B et C"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Patient de 35 se présente avec une névrite cervico-brachial droite à prédominance nocturne. La douleur irradie jusqu’au pouce, il n’y a pas de déficit moteur. Les Mouvements du cou n’entrainent aucune douleur. Un scanner sans injection est normal. Comment confirmez vous votre hypothèse ?",
					questions: [
						"Par une IRM",
						"Par une RX",
						"Par un CT avec injection",
						"A et C",
						"B et C"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Patient de 35 se présente avec une névrite cervico-brachial droite à prédominance nocturne. La douleur irradie jusqu’au pouce, il n’y a pas de déficit moteur. Les Mouvements du cou n’entrainent aucune douleur. Un scanner sans injection est normal.  Quel type de traitement proposeriez vous ?",
					questions: [
						"analgésique uniquement",
						"chirurgical",
						"par péridural",
						"A et B",
						"B et C"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel racine nerveuse est atteinte ?",
					questions: [
						"C5",
						"C6",
						"C7",
						"C8",
						"Th1"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel réflexe pourrait être diminué ou aboli ?",
					questions: [
						"Tricipital (=C7)",
						"Bicipital (mais C est juste aussi...)",
						"Humero-stylo-radial",
						"Hoffmann"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Suite à un accident de roulage sans ceinture de sécurité, un patient arrive aux urgence avec un GCS 5/15, mydriase droite fixe et une hémiparésie droite. Le patient est inconscient directement. Quel est la prise en charge que vous proposez ?",
					questions: [
						"Manœuvre « ABC »",
						"Examen physique complet",
						"Examen neurologique complet",
						"Réalisation d’une imagerie",
						"A,B,D"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Comment expliquez-vous la mydriase et l’hémiparésie ?",
					questions: [
						"Engagement sous falcoriel",
						"Engagement du gyrus frontal droit",
						"Compression du tronc cérébrale par l’amygdale droite",
						"Compression du tronc cérébrale par l’amygdale gauche",
						"Aucune proposition"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Comment expliquer l’hémiparésie ipsilatérale ?",
					questions: [
						"Lésion faisceau pyramidal après la décussation",
						"Lésion faisceau pyramidale par l’uncus droit",
						"Lésion faisceau pyramidal par la tente du cervelet",
						"Lésion faisceau pyramidal par l’uncus gauche",
						"Lésion faisceau pyramidal par le nerf III"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "L’imagerie démontre un hématome de type convexe et concave épousant bien le cerveau et spontanément hyperdense. De quoi s’agit-il ?",
					questions: [
						"Un hématome sous-dural aigue",
						"Un hématome sous-dural chronique",
						"Un hématome extra-dural",
						"Un hématome intra parenchymateux",
						"Un hématome sous arachnoïdien"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Son pronostique dépend",
					questions: [
						"La vitesse de prise en charge",
						"Àge du patient (Rappel : HSDA pronostic dépend de l’âge versus HED ou ca dépend aussi rapidité de la prise en charge)",
						"La valeur de TA",
						"La valeur de PIC",
						"A et B"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un enfant de 10 ans présente une perte de poids associé à des nausées et des vomissement depuis plus de 15 jours. Il a par ailleurs un torticolis depuis plus d’un mois traité par ostéopathie Quel est votre hypothèse diagnostique ?",
					questions: [
						"Gastro-entérite",
						"Lésion cérébrale",
						"Lésion cérébelleuse",
						"Infection virale",
						"AetB"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quelle mise au point effectuez vous ?",
					questions: [
						"Une RX du crâne",
						"Un CT sans contraste",
						"Un CT avec contraste",
						"IRM",
						"C ou D"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Quelle est la tumeur la plus fréquente chez les enfants ?",
					questions: [
						"Glioblastome",
						"Astrocytome pilocytique",
						"Adénome hypophysaire",
						"Médulloblastome",
						"Edendynome"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Comment expliquez vous le torticolie?",
					questions: [
						"Contracture du SCM",
						"Irritation du nerf spinal",
						"Descente de l’amygdale cérébelleuse",
						"A,B,C",
						"A et B"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Le pronostic de cet enfant dépend de:",
					questions: [
						"Nature histologique",
						"Â de l’enfant",
						"Qualité de résection",
						"A et B",
						"A et C"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Un patient présente une marche à petit pas, des troubels de la mémoire, une incontinence urinaire. L’imagerie montre une hydrocéphalie. Aucune lésion cérébrale n’a été mise en évidence. Comment mesurez-vous la pression intracrânienne ?",
					questions: [
						"Par PL en position couchée L1-L2",
						"Par PL en position assise en L2-L3",
						"Par PL en position assise en L3-L4",
						"Par PL en position couchée en L4-L5",
						"B et D"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Hydrocéphalie peut avoir comme origine",
					questions: [
						"Troubles de la résorption",
						"Une hypersécrétion",
						"Des troubles d’écoulement du LCR",
						"A et B",
						"A et C"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Que prédit la loi de Monro Kellie ?",
					questions: [
						"Que le volume intracrânien et constant",
						"Que la modification d’une des volumes entraine une modification des autres",
						"Que chez les enfants le volume peut se modifier",
						"A et B",
						"A et C"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "L’hydrocéphalie se traite",
					questions: [
						"Par une ventriculostomie",
						"Par une dérivation ventriculo-péritonéale",
						"Par une dérivation externe",
						"A et B",
						"A, B et C"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Une hydrocéphalie post HSA est liée",
					questions: [
						"Au clippage de l’anévrisme",
						"A l’embolisation",
						"Au produit de dégradation du sang",
						"A et C",
						"B et C"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Patient de 36 ans, accident sur la voie publique sans ceinture de sécurité, sans perte de connaissance, mais traumatisme crânien. Arrivé aux urgences : GCS 12/15, parésie de l’hémicorps droit et asymétrie des pupilles (gauche>droite). Quelle est la mise point que vous pratiquez",
					questions: [
						"RM",
						"Rx Crâne",
						"Ct Crâne",
						"EEG",
						"C et D"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "L’examen clinique peut être expliqué par",
					questions: [
						"Hématome sous dural aigu",
						"Hématome sous dural chronique",
						"Hématome extra dural",
						"Un engagement sous falcoriel",
						"Un engagement amygdalien"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Du point de vue hémodynamique on doit trouver",
					questions: [
						"PPC N, DSC N, TA augmenté",
						"PPC diminué, DSC N, TA augmenté",
						"PPC augmenté, DSC N, TA augmenté",
						"PPC N, DSC N, TA diminué",
						"PPCN, DSCN, TAN"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "le pronostic est fonction de :",
					questions: [
						"La pathologie",
						"De l’âge",
						"De la rapidité de la prise en charge",
						"Des modifications hémodynamiques",
						"A,B,C et D"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Un patient vient à votre consultation, il présente un steppage à la marche de la jambe droite, votre examen clinique démontre entre autres un Lasègue + à 45°. Quelle mise au point demandez vous ?",
					questions: [
						"EMG",
						"CT colonne",
						"IRM",
						"EEG",
						"A et B"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Est-ce une urgence ?",
					questions: [
						"oui",
						"oui car le Lasègue est positif",
						"oui car l’EMG est positif",
						"non",
						"non car le Lasègue est positif"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quel traitement conseillez vous ?",
					questions: [
						"Anti-inflammatoire",
						"Analgésique",
						"Péridurale",
						"Chirurgie",
						"a,b,c"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Patiente de 53 ans, aucun antécédent personnel et familial. Elle présente une céphalée qu’elle décrit comme la plus horrible de sa vie. L’examen clinique démontre une raideur de nuque, une photophobie et une hémiparésie droite. Quel est le 1er examen à réaliser en urgence IRM cérébrale",
					questions: [
						"IRM cérébrale",
						"Ponction lombaire",
						"IRM lombaire",
						"Ct Scanner",
						"Angiographie cérébrale"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quel est le grade clinique WFNS de la patiente ?",
					questions: [
						"1",
						"2",
						"3",
						"4",
						"5"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un anévrisme est diagnostiqué. On sait que :",
					questions: [
						"Les anévrismes les plus fréquents sont situés en circulation postérieure",
						"Les anévrismes de l’artère carotide interne sont plus fréquents que les anévrismes sylviens",
						"Les anévrismes sont le plus souvent situés à distance du polygone de Willis",
						"Les anévrismes de l’artère communicante antérieure sont peu fréquents.",
						"Les anévrismes péri calleux ne saignent pas"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "La patiente est traitée et sa sortie est prévue. Vous lui recommandez",
					questions: [
						"La prise de statines",
						"L’arret du tabac et un contrôle de sa tension artérielle",
						"L’arrêt de toute activité sportive",
						"Un dépistage pour ses enfants",
						"Un dépistage de son conjoint"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un nouveau né présente une lésion au niveau de bas du dos. Elle est rénitente, avec un plaque centrale, une béance anale, des pieds en équin, une plégie des membres inférieurs. Aucune surveillance n’avait été fait durant la grossesse. Ce nouveau né présente :",
					questions: [
						"Méningocéle",
						"Encéphalocèle",
						"Myéloméningocèle",
						"Spina bifida occulta",
						"C et D"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La prise en charge doit être",
					questions: [
						"Médicale",
						"chirurgicale",
						"radiologique",
						"Urologique",
						"Multidisciplinaire (neurochir, uro, ortho, kiné, psy..)"
					],
					answer: null,
					rightAnswer: 5,
					result: null
				},
				{
					title: "Dans les jours qui suivent sont traitement, le périmètre crânien décroche de sa courbe, et il présente des yeux en couché de soleil. Que développe t’il ?",
					questions: [
						"Une méningite",
						"Une hémorragie cérébrale",
						"Une hydrocéphalie",
						"Une hypertension intracrânienne",
						"Une craniosténose"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un jeune adulte présente des absences de façon quotidienne. Dans ses antécédents, on retrouve, une méningite et des convulsions hyperthermiques. Quelle mise au point réalisez vous ?",
					questions: [
						"Ct Scan Cérébrale",
						"IRM cérébrale",
						"Un EEG",
						"Une ponction lombaire",
						"A et B"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Sans lésion cérébrale et le traitement médical ne permet pas de contrôler ses absences que peut on lui proposer",
					questions: [
						"Une biopsie cérébrale",
						"Une stimulation cérébrale profonde",
						"Stimulation du nerfs vague",
						"Une hémisphérotomie",
						"Une lésionectomie"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Avec une lésion cérébrale visible à l’imagerie qui fait penser à une lésion primitive du cerveau, vous proposez",
					questions: [
						"Une chimiothérapie",
						"Une radiothérapie",
						"Une biopsie",
						"Une résection",
						"C et D"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un patient vous consulte avec des céphalées matinales en augmentation, des troubles visuels (diminution des champs visuels en bitemporal). Dans les antécédents, il y a un canal carpien bilatéral. L’examen clinique démontre un patient prognathe, de grandes mains. Votre hypothèse diagnostique est :",
					questions: [
						"Une hypertension intracrânienne",
						"Un adénome hypophysaire",
						"Une tumeur cérébrale maligne",
						"A et C",
						"A et B"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Les céphalées a prédominance matinale sont liés à",
					questions: [
						"la tension artérielle",
						"la pression osmotique du sang",
						"l’hydrocéphalie",
						"la présence des protéines dans le sang",
						"tout est juste"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Dans le cadre des adénomes hypophysaires, les plus fréquent et le plus fréquemment traiter chirurgicalement sont",
					questions: [
						"à sécrétion de prolactine",
						"à sécrétion d’ACTH",
						"sans sécrétions",
						"à sécrétion de TSH"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Les vertiges d’origine centrale :",
					questions: [
						"Ne sont pas accompagnées par des troubles d’équilibre",
						"Sont rarement accompagnées par des nausées",
						"Peuvent être accompagnées par un nystagmus qui change de direction selon la direction du regard",
						"La lésion sous-jacente est le plus souvent dans le cervelet ou le tronc cérébral."
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Dans l’atteinte unilatérale totale du nerf VII :",
					questions: [
						"Les rides du front sont effacées",
						"L’œil est fermé",
						"Il y a une baisse d’audition du même coté",
						"Il y a une agueusie des deux tiers postérieurs de la langue"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. L’atteinte des faisceaux spino-thalamiques cause :",
					questions: [
						"Une diminution des réflexes myotatiques",
						"Un signe de Romberg positif",
						"Une perte de sensibilité thermique",
						"Une perte de graphesthésie"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un patient de 60 ans se plaint de difficulté croissante aux mouvements fins de la main droite, associée à une sensation de raideur du membre supérieur droit. L’examen neurologique montre une diminution du ballant du membre supérieur droit durant la marche, un ralentissement des mouvements de la main droite et une légère rigidité en roue dentée des membres de droite. Il n’y a aucun mouvement involontaire. Le patient dit avoir mal à l’épaule droite et se sentir fatigué. Son humeur est dépressive. Quel est le diagnostic plus probable parmi :",
					questions: [
						"Cervico-brachialgie droite",
						"Maladie de Parkinson",
						"Canal cervical étroit"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. La maladie d’Alzheimer est caractérisée par :",
					questions: [
						"Un enchevêtrement intra-neuronal de filaments de protéine tau hyperphosphorylée",
						"Une diminution de l’activité métabolique corticale à la tomodensitométrie à émission de positons (PET scan).",
						"Une augmentation de la concentration de beta-amyloïde dans le liquide céphalo-rachidien",
						"Des dépôts d’amyloïde dans la paroi des petits vaisseaux sanguins"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un enfant de 9 ans développe des troubles de la marche et d’équilibre lentement progressifs et devient de plus en plus maladroit. Le seul autre symptôme est une scoliose. À l’examen neurologique on constate une ataxie axiale et segmentaire, une diminution de la perception du diapason aux malléoles, l’absence des réflexes myotatiques aux quatre membres et un signe de Babinski bilatéral. L’anamnèse familiale est négative. Quel est le diagnostic plus probable ?",
					questions: [
						"Maladie de Wilson",
						"Ataxie de Friedreich",
						"Tumeur du cervelet",
						"Sclérose en plaques"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un homme de 47 ans arrive aux urgences en état confusionnel évoluant depuis 36 heures. Sa température est 39°. Il fait une crise d’épilepsie tonico-clonique. La nuque est raide. Un CT scan sans contraste est dans le limites de la normale, en particulier il ne montre pas de lésion occupant de l’espace. La ponction lombaire montre : protéines 84 mg/l, glucose 68 mg/dl (glycémie 99 mg/dl), 110 GB/μl (98% lymphocytes) et 230 GR/μl. Quelle est la meilleure approche à suivre :",
					questions: [
						"Le diagnostic plus probable est une méningite virale. Il faut donner un traitement de soutien en attendant les résultats des cultures et PCR.",
						"Le diagnostic plus probable est une encéphalite herpétique. Il faut faire une IRM car le CT ne montre pas d’anomalie des lobes temporaux.",
						"Le diagnostic plus probable est une méningite bactérienne. Il faut donner un traitement antibiotique par céphalosporine de 3ème génération en attendant les résultats des cultures et PCR.",
						"Le diagnostic plus probable est une encéphalite herpétique. Il faut commencer le traitement par acyclovir en attendant les résultats des autres examens complémentaires, y compris cultures et PCR"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. En cas d’ischémie cérébrale, dans la pénombre :",
					questions: [
						"Il y a une restriction de la diffusion des molécules d’eau, révélée par l’IRM de diffusion",
						"Le CT de perfusion montre que la perfusion est réduite entre 8 et 18 ml/100g/min mais le volume sanguin n’est pas diminué",
						"Le tissu cérébral garde temporairement son intégrité structurelle",
						"Les neurones deviennent hypo-excitables",
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. La thrombolyse est indiquée dans le traitement de l’AVC ischémique aigu si :",
					questions: [
						"Le CT ne montre pas d’hémorragie",
						"Le patient n’est pas anticoagulé",
						"Le patient n’a pas une hypertension non contrôlée (pression artérielle systolique >185 ou diastolique >110).",
						"Le début des symptômes remonte à moins de 6 heures"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Parmi les suivants, quel symptôme n’est pas typique d’un AIT :",
					questions: [
						"L’amaurose fugace (perte visuelle monoculaire transitoire)",
						"L’hémianopsie homonyme",
						"L’hémianopsie bitemporale",
						"La diplopie"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Parmi les suivants, quel signe ou symptôme n’est pas typique de la sclérose latérale amyotrophique :",
					questions: [
						"Dysphagie",
						"Crampes musculaires",
						"Signe de Babinski",
						"Incontinence urinaire"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Les structures suivantes font partie du circuit de Papez, impliqué dans la mémoire à long terme :",
					questions: [
						"Gyrus parahippocampique",
						"Cortex occipital",
						"Noyau antérieur du thalamus",
						"Cortex cingulaire"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Une lésion du cortex associatif pariétal droit peut entraîner :",
					questions: [
						"Troubles de la reconnaissance des couleurs",
						"Héminégligence",
						"Apraxie de l’habillage",
						"Anosognosie"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. La maladie de Huntington :",
					questions: [
						"Est liée au chromosome X",
						"Est due à l’expansion d’une répétition CAG",
						"La chorée est toujours le premier symptôme",
						"Le cortex n’est pas atrophié"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un homme de 70 ans arrive aux urgences pour une perte visuelle aigue à l’œil droit. À l’anamnèse on apprend que depuis 2 mois il se sent fatigué et il a des douleurs au cou et aux épaules. Depuis 15 jours il a aussi une douleur vive dans la région temporale droite, intermittente au début puis devenue continue. Quel est le diagnostic plus probable :",
					questions: [
						"Thrombose de la carotide droite",
						"Dissection de la carotide droite",
						"Artérite de Horton (artérite temporale)",
						"Céphalée de Horton (cluster headache - céphalée en grappes)"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un homme de 25 ans est inquiet à cause d’épisodes répétés de trouble visuel. Il décrit une image lumineuse à zigzag au centre du champ visuel, qui ensuite s’élargit et traverse tout un hémichamps visuel jusqu’à a sa périphérie, en environs 20 minutes. Le diagnostic plus probable est :",
					questions: [
						"AIT",
						"Crise d’épilepsie focale",
						"Crise d’anxiété",
						"Equivalent migraineux"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Le score Glasgow d’un patient qui ouvre les yeux sur ordre verbal, chasse les stimulus nociceptifs et a une conversation confuse est de :",
					questions: [
						"10",
						"11",
						"12",
						"13"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un patient qui bouge, a les yeux ouverts mais ne parle pas et n’exécute pas les ordres a probablement :",
					questions: [
						"Une aphasie globale",
						"Un état psychotique",
						"Une intoxication aigue",
						"Les trois sont possibles"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Dans la mise au point d’un état confusionnel aigu, l’EEG est parfois essentiel pour exclure :",
					questions: [
						"Une aphasie globale",
						"Un état de mal épileptique non convulsivant",
						"Une intoxication aigue",
						"Un AVC"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "La cause plus fréquente d’épilepsie de nouvelle installation chez la personne âgée est :",
					questions: [
						"Séquelle d’AVC corticale",
						"Maladie d’Alzheimer",
						"Tumeur cérébrale",
						"Démence à corps de Lewy",
						
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un patient qui fait une crise d’épilepsie inaugurale (première fois dans sa vie) sur sevrage alcoolique :",
					questions: [
						"Doit être traité par antiépileptiques pour 6 mois",
						"Ne doit pas être traité par antiépileptiques car sa crise est provoquée",
						"Doit être traité par antiépileptiques pour 1 mois",
						"Doit être traité par antiépileptiques à vie"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un patient qui a subi un trama crânien sévère avec pétéchies intra-parenchymateuses :",
					questions: [
						"Doit être traité par antiépileptiques pour 6 mois pour prévenir les crises tardives",
						"Ne doit pas être traité par antiépileptiques",
						"Doit être traité par antiépileptiques pour 1 semaine pour prévenir les crises précoces",
						"Doit être traité par antiépileptiques à vie"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Le but du traitement antiépileptique doit être :",
					questions: [
						"La réduction de 50% de la fréquence des crises",
						"La réduction de 80% de la fréquence des crises",
						"La réduction de la fréquence des crises à moins de 1 par mois",
						"Être libre de crises"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Une femme de 28 ans, trois jours après avoir accouché, développe une céphalée holocrânienne, intermittente au départ, puis continue et accompagnée par nausées. Les jours suivants, une hémianopsie homonyme droite et une faiblesse de l’hémicorps droit apparaissent. Finalement, après une crise d’épilepsie, elle arrive aux urgences. Quel est le diagnostic plus probable :",
					questions: [
						"Thrombose de sinus veineux",
						"AVC sylvien gauche",
						"Sclérose en plaques",
						"Hémorragie cérébrale sur hypertension artérielle"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un homme de 31 ans depuis 3 ans se plaint de faiblesse et raideur aux membres inférieurs, qui ont évolué jusqu’à limiter son périmètre de marche a moins de 200m. Durant la dernière année il a développé une urgence urinaire. Depuis 1 mois, il a une diplopie quand il regarde vers la droite. L’examen neurologique montre : une faiblesse globale des membres inférieurs, avec réflexes myotatiques très vifs et signe de Babinski bilatéral ; une perte de sensibilité vibratoire aux malléoles, bilatérale, et une perte de sensibilité tactile à l’hémicorps gauche ; au regard vers la droite il y a un déficit d’adduction de l’œil gauche et des secousses de nystagmus à droite. L’IRM montre plusieurs lésions hypersignal T2 périventriculaires, dans le corps calleux, dans le tronc cérébral et dans la moelle cervicale. La ponction lombaire révèle des IgG oligoclonales dans le LCR. Le diagnostic est :",
					questions: [
						"Sclérose en plaques à poussées-rémissions",
						"Maladie de Lyme",
						"Sclérose en plaques primaire progressive",
						"Sclérose en plaques secondaire progressive"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un homme de 31 ans depuis 3 ans se plaint de faiblesse et raideur aux membres inférieurs, qui ont évolué jusqu’à limiter son périmètre de marche a moins de 200m. Durant la dernière année il a développé une urgence urinaire. Depuis 1 mois, il a une diplopie quand il regarde vers la droite. L’examen neurologique montre : une faiblesse globale des membres inférieurs, avec réflexes myotatiques très vifs et signe de Babinski bilatéral ; une perte de sensibilité vibratoire aux malléoles, bilatérale, et une perte de sensibilité tactile à l’hémicorps gauche ; au regard vers la droite il y a un déficit d’adduction de l’œil gauche et des secousses de nystagmus à droite. L’IRM montre plusieurs lésions hypersignal T2 périventriculaires, dans le corps calleux, dans le tronc cérébral et dans la moelle cervicale. La ponction lombaire révèle des IgG oligoclonales dans le LCR.Le déficit oculomoteur présenté par ce patient est :",
					questions: [
						"Ophtalmoplégie internucléaire",
						"Parésie du nerf III gauche",
						"Nystagmus évoqué par le regard (gaze-evoked)",
						"Parésie du nerf III gauche et du nerf VI droit"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un homme de 72 ans, diabétique et avec antécédents de cardiopathie ischémique et multiples AIT, développe sur plusieurs semaines des douleurs dans la région lombaire, puis dans la hanche et la cuisse gauche, suivies par une amyotrophie et une faiblesse asymétrique atteignant la ceinture pelvienne du côté gauche. Le diagnostic plus probable est :",
					questions: [
						"AVC ischémique médullaire",
						"Amyotrophie diabétique",
						"Artériopathie iliaque",
						"Hernie discale lombaire"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Une femme de 78 ans présente depuis quelques mois des troubles de mémoire, somnolence, irritabilité. Depuis trois jours elle a du mal à se tenir débout et fait des chutes à répétition. Elle se plaint de paresthésies aux membres inférieurs. À l’examen neurologique un ralentissement psychomoteur est évident. La patiente est capable de se tenir débout avec difficulté, à cause de faiblesse des membres inférieurs. Elle perte soudainement l’équilibre à la fermeture des yeux. Le diapason n’est pas aperçu aux membres inférieurs. Les réflexes myotatiques sont très faibles aux membres inférieurs, avec des réponses cutanées plantaires indifférentes. La prise de sang montre une anémie macrocytique. Le diagnostic plus probable est :",
					questions: [
						"Maladie d’Alzheimer débutante",
						"Déficit de vitamine B12",
						"Alcoolisme avec déficit de vitamine B1",
						"Encéphalomyélite autoimmune"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Le syndrome de West",
					questions: [
						"Survient après 1 an",
						"S’accompagne de crises d’épilepsie dont la sémiologie est variable",
						"S’accompagne toujours d’une lésion cérébrale à l’IRM",
						"Nécessite parfois un traitement par corticoides à hautes doses"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quelle est la présentation clinique la plus fréquente lorsque vous examinez un enfant âgé de 3 mois qui a présenté un AVC sylvien congénital droit massif ?",
					questions: [
						"Troubles neurovisuels",
						"Hémiplégie gauche + aphasie",
						"Hémiplégie gauche",
						"L’examen neurologique peut-être normal, hormis une légère hypotonie axiale"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. La myopathie de Duchenne",
					questions: [
						"Est visible cliniquement dès la naissance",
						"Se manifeste par un tableau relativement stable",
						"Touche uniquement les garçons",
						"S’accompagne parfois d’une élévation des CK"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la bonne réponse.",
					questions: [
						"La leucodystrophie métachromatique s’accompagne d’un syndrome pyramidal aboutissant à la perte de la marche vers l’âge de 18 mois",
						"S’accompagne toujours d’une hyperréflexie",
						"N’est pas une maladie dégénérative",
						"La régression motrice et cognitive est secondaire à une accumulation intralysosomiale de sphingolipides essentiellement dans les cellules du cortex cérébral"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Dans le syndrome de Guillain Barré :",
					questions: [
						"Le traitement par plasmaphérèse est aussi efficace qu’un traitement par immunoglobulines intraveineuses",
						"Le déficit moteur maximal est atteint le plus souvent 6 semaines après le début des symptômes",
						"Le syndrome comprend plusieurs variantes pouvant toucher la gaine de myéline ou l’axone du nerf périphérique",
						"Le déficit moteur est accompagné d’une perte des réflexes achilléens"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Les neuropathies associées aux gammapathies monoclonales :",
					questions: [
						"En présence d’un anticorps anti MAG sont souvent associées à la présence d’une paraprotéine de type IgG",
						"Sont souvent caractérisées par une atteinte démyélinisante",
						"Sont le plus souvent associées à une paraprotéine de type IgA",
						"Touchent le plus souvent des patients de moins de 30 ans"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Vous suspectez une atteinte radiculaire chez un patient, lors de la prise des réflexes à l’occasion de la réalisation de son examen neurologique :",
					questions: [
						"La perte d’un réflexe achilléen suggère une atteinte de la racine L5",
						"La perte du réflexe tricipital suggère une atteinte de la racine C8",
						"La perte du réflexe rotulien suggère une atteinte de la racine L4",
						"La perte du réflexe bicipital suggère une atteinte de la racine C7",
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. La sclérose latérale amyotrophique :",
					questions: [
						"Peut toucher tous les muscles du corps : bras, jambes, diaphragme, oculomoteurs...",
						"Peut se caractériser en cas d’atteinte du motoneurone supérieur par la présence d’une hyperréflexie",
						"Est associée à la présence d’une névrite optique détectable aux potentiels évoqués visuels",
						"Est liée à la présence exclusive d’une atteinte du motoneurone supérieur"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Lors de la réalisation de potentiels évoqués somesthésiques on s’attend à observer une disparition du potentiel N13 dans les situations suivantes :",
					questions: [
						"L’anoxie cérébrale",
						"Une tumeur de la région thalamique",
						"La syringomyélie",
						"Une tumeur de la région bulbaire"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Dans un électroencéphalogramme :",
					questions: [
						"L’activité alpha correspond à des activités dont la gamme de fréquence oscille entre 5 et 16 Hz",
						"La survenue de courants post synaptiques inhibiteurs au niveau du soma des neurones corticaux va être enregistrée comme une déflection positive au niveau des électrodes de surface",
						"La survenue de courants post synaptiques excitateurs au niveau des dendrites des neurones corticaux va être enregistrée comme une déflection positive au niveau des électrodes de surface",
						"L’enregistrement des activités électriques correspond à des courants extracellulaires produits par la sommation de l’activité de nombreux neurones corticaux"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. Dans le syndrome de Lambert-Eaton :",
					questions: [
						"Le syndrome s’accompagne d’anticorps dirigés contre le canal sodium voltage dépendant",
						"Le syndrome se caractérise par une diminution du relargage de glutamine dans la fente synaptique",
						"Le traitement par 3-4 DAP bloque les canaux potassium présynaptiques",
						"La facilitation post exercice est liée à une accumulation du calcium dans la terminaison post synaptique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la bonne réponse. La maladie de Charcot :",
					questions: [
						"Est caractérisée par des vitesses de conduction nerveuses très ralenties",
						"Est liée à une atteinte des motoneurones supérieurs et inférieurs",
						"Est liée à une délétion du gène codant pour la protéine PMP22",
						"Est liée à une anomalie de la jonction neuromusculaire"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Dans la myasthénie :",
					questions: [
						"Son exploration neurophysiologique démontre lors de la stimulation à 3 Hz la présence d’un décrément de plus de 10% en amplitude",
						"La maladie peut s’accompagner d’anticorps dirigés contre le récepteur à acétylcholine",
						"Le traitement par pyridostigmine facilite l’activité de la cholinestérase",
						"Le froid améliore la transmission au niveau de la jonction neuromusculaire."
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse. Dans les myosites :",
					questions: [
						"Les patients présentent souvent une faiblesse musculaire proximale",
						"Les patients présentent souvent des douleurs musculaires, parfois une sensibilité à la palpation des muscles",
						"L’électromyographie ne démontre pas d’activités spontanées dans la mesure où ces anomalies reflètent une atteinte neurogène",
						"La biopsie musculaire démontre souvent un infiltrat de cellules inflammatoires périvasculaires"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "À propos du concept de « compression de la morbidité », quelle est la proposition exacte ?",
					questions: [
						"C’est une hypothèse qui tend à expliquer l’allongement de la longévité de l’espèce humaine",
						"C’est une hypothèse qui tend à expliquer l’espérance de vie à la naissance",
						"C’est une hypothèse qui propose de retarder l’apparition des incapacités au cours de la vie",
						"C’est une hypothèse validée pour l’ensemble de la population mondiale."
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La « fragilité » en gériatrie est :",
					questions: [
						"une maladie du vieillissement",
						"répartie de manière homogène dans la population",
						"un état qui combine le vieillissement, les maladies chroniques et des facteurs de risque exposant le sujet à un risque d’apparition d’incapacité fonctionnelle",
						"un état non modifiable"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La « présentation atypique » du « patient gériatrique » est le reflet de :",
					questions: [
						"une perception différente du sujet âgé de la maladie",
						"la présence de symptômes parfois dissimulés ou minimisés",
						"une physiopathologie différente des sujets jeunes",
						"toutes ces propositions sont exactes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "À propos du risque accru de pathologies iatrogènes en gériatrie, on trouve comme facteurs de risque",
					questions: [
						"la non connaissance des prescripteurs de la liaison des médicaments à l’albumine sérique",
						"l’augmentation du volume de distribution hydrique chez le sujet âgé",
						"la monopathologie",
						"l’utilisation de l’échelle STOPP"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Le « syndrome gériatrique » :",
					questions: [
						"est un modèle linéaire qui associe une cause, un mécanisme physiopathogénique expliquant un ensemble de manifestations cliniques",
						"voit sa prévalence diminuer avec l’âge",
						"est plurifactoriel et le reflet de la fragilité",
						"se présente le plus fréquemment comme un syndrome Cushing"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "À propos de l’évaluation gériatrique globale quelle est la proposition correcte ?",
					questions: [
						"c’est une évaluation complète de l’ensemble des organes de l‘individu",
						"c’est une évaluation qui intègre les aspects médicaux, psycho-sociaux, fonctionnels et de l’environnement du patient",
						"c’est une évaluation des troubles cognitifs associée à une prise en charge palliative",
						"c’est une évaluation sociale effectuée par le travailleur social"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "À propos de l’évaluation gériatrique globale quelle est la proposition correcte ?",
					questions: [
						"Elle n’a pas d’effet sur la mortalité",
						"Elle permet d’augmenter le nombre d’institutionnalisation",
						"Elle augmente le nombre de ré hospitalisations",
						"Elle améliore la récupération fonctionnelle"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "L’effet domino représente",
					questions: [
						"L’augmentation de l’incidence des chutes au cours du vieillissement",
						"Le fait d’administrer un médicament pour diminuer l’effet secondaire d’un autre médicament déjà prescrit",
						"N’a pas d’effet sur la poly médication",
						"Est un concept de base dans l’utilisation de l’échelle START"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "La formule de Cockcroft est (âge en années, poids en Kg, créatinine plasmatique en mg/100mL):",
					questions: [
						"(140-âge) x poids/72 x créatinine. (x 0.85 pour le genre féminin)",
						"(140-âge) x poids/82 x créatinine. (x 0.55 pour le genre féminin)",
						"(170-âge) x poids/52 x créatinine. (x 0.55 pour le genre féminin)",
						"(140-âge) x poids/62 x créatinine. (x 0.55 pour le genre féminin)"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "À propos de l’échelle de Katz, quelle est la proposition correcte ?",
					questions: [
						"Cette échelle évalue l’autonomie intellectuelle d’un individu",
						"Cette échelle évalue les capacités des activités instrumentales de la vie journalière",
						"Cette échelle évalue les capacités cognitives",
						"Cette échelle évalue les activités de base de la vie quotidienne"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "À propos de l’échelle Mini Mental Status Evaluation (MMSE) qu’elle est la proposition exacte ?",
					questions: [
						"elle évalue la dépendance fonctionnelle",
						"elle est scorée sur 40 points",
						"son interprétation doit tenir compte du niveau socioculturel du patient",
						"c’est une échelle diagnostique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle est la prévalence de la dénutrition au sein d’une unité de gériatrie ?",
					questions: [
						"5%",
						"15%",
						"40%",
						"plus de 50%"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "À propos de l’évaluation de la douleur chez les sujets non communiquant, quelle échelle peut être utilisée ?",
					questions: [
						"Echelle de Cornell",
						"CAM",
						"Doloplus",
						"GDS"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "À propos du risque de dénutrition en gériatrie quelle est la proposition fausse ?",
					questions: [
						"Il existe une trouble du métabolisme énergétique apportant une vulnérabilité par rapport au maintien l’équilibre calorique en cas de jeûne",
						"La polymédication constitue un facteur de risque de dénutrition",
						"Le syndrome inflammatoire s’associe à une perte d’appétit",
						"La dépression n’est pas un facteur de risque de refus alimentaire"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Parmi les médicaments ci-dessous, lesquels sont potentiellement anorexigènes ?",
					questions: [
						"les inhibiteurs de recaptation de la sérotonine (SSRI)",
						"les opiacés",
						"la digoxine",
						"toutes les propositions sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Face à une démence terminale incapable d’assurer son hydratation et sa nutrition, que proposez-vous pour traiter les symptômes liés à la déshydratation et la dénutrition ?",
					questions: [
						"Une nutrition entérale par sonde",
						"La mise en place d’une gastrostomie percutanée",
						"Une hydratation par voie intraveineuse",
						"Une prise en charge palliative sans hydratation et/ou nutrition artificielle"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Parmi les critères DSM IV du delirium, quel est l’élément qui permet, en dehors d’une éventuelle maladie à corps de Lewy, de préciser le diagnostic d’un état confusionnel aigu ?",
					questions: [
						"Un trouble de la vigilance et le caractère fluctuant des troubles",
						"L’altération cognitive",
						"Des troubles du sommeil",
						"La présence d’hallucinations visuelles"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "À propos du delirium (état confusionnel aigu) quelle est la proposition exacte ?",
					questions: [
						"C’est le premier diagnostic à évoquer chez une personne âgée institutionnalisée si l’examen microscopique d’urine est perturbé",
						"Il peut représenter l’expression d’une fragilité cognitive liée à une démence sous-jacente débutante",
						"Il est bien souvent d’origine monofactorielle",
						"Il n’existe pas de facteurs de risque connus"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "À propos du statut épileptique non myoclonique (Non Convulsivant Status Epilepticus)",
					questions: [
						"peut se présenter comme un delirium",
						"s’il se confirme, il ne faut plus pratiquer de ponction lombaire en cas de syndrome infectieux non identifié",
						"ne partage pas de facteurs de risque commun au delirium",
						"ne doit pas être recherché en cas de delirium prolongé"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Que proposez-vous dans l’immédiat en cas de découverte d’une rétention vésicale aigue chez un homme de 77 ans?",
					questions: [
						"La mise en place d’un cathéter à demeure et une surveillance de la diurèse",
						"Une évaluation neuropsychologique",
						"Un examen neurologique, une toucher rectal, un sondage in/out avec EMU et culture, une revue des médicaments",
						"Vous doser les tests de coagulation et vous appelez l’urologue pour une résection endoscopique de la prostate"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse parmi les affirmations suivantes:",
					questions: [
						"Dans la rétine une cellule ganglionnaire centre ON va s’activer de manière préférentielle si le centre du champ récepteur est éclairé",
						"Le potentiel N20 lors de la réalisation de potentiels évoqués somesthésiques par stimulation du nerf médian reflète les activités électriques du lemniscus médian",
						"La stimulation par pattern dans les potentiels évoqués visuels utilise la détection des changements de contraste pour générer les activités électriques enregistrées",
						"Une atteinte centromédullaire cervicale peut entrainer une altération du potentiel N13 lors de la réalisation de potentiels évoqués somesthésiques par stimulation du nerf médian"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse : dans une atteinte du nerf périphérique de type démyélinisante",
					questions: [
						"La durée des réponses motrices peut être anormalement augmentée (dispersion temporelle)",
						"La latence distale des réponses motrices peut être anormalement courte",
						"Les vitesses de conductions peuvent être très ralenties",
						"Lors d’une lésion focale de la gaine de myéline une stimulation proximale (au-dessus du site lésionnel) peut donner une chute d’amplitude des réponses motrices enregistrées au niveau distal sur le muscle étudié."
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quelle est la proposition vraie à propos du syndrome de Guillain-Barré (polyradiculonévrite inflammatoire démyélinisante aigüe) :",
					questions: [
						"est toujours associé à une infection à Campylobacter jejuni",
						"se traite par administration de corticostéroïdes",
						"est classiquement associé au diabète",
						"se traite par administration d’immunoglobulines par voie intraveineuse."
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quelle est la proposition erronée – Dans le syndrome du canal carpien :",
					questions: [
						"L’atrophie de l’éminence thénar apparaît souvent tardivement",
						"Les plaintes de douleurs nocturnes sont fréquentes",
						"Les plaintes de faiblesse de la main concernée sont classiques",
						"l’électromyographie montre classiquement des signes de dénervation dans le muscle premier interosseux dorsal"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Trouvez la mauvaise proposition concernant les neuronopathies motrices :",
					questions: [
						"L’électromyographie est anormale dans la sclérose latérale primaire",
						"La sclérose latérale amyotrophique est caractérisée par une atteinte des neurones moteurs supérieurs et inférieurs",
						"L’amyotrophie spinale progressive est caractérisée par une atteinte du motoneurone inférieur",
						"La sclérose latérale primaire est caractérisée par la présence d’une spasticité, de réflexes vifs"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Trouvez la bonne réponse concernant les pathologies de la jonction neuromusculaires :",
					questions: [
						"Dans le Lambert Eaton la durée optimale de contraction musculaire pour observer la facilitation post exercice est de 30 secondes.",
						"Dans le Lambert Eaton le traitement par 3 -4 di amino pyridine bloque les canaux calcium",
						"La myasthénie est caractérisée par une faiblesse touchant principalement les muscles proximaux",
						"La myasthénie est souvent accompagnée d’une dysautonomie liée à l’atteinte des récepteurs à acétylcholine"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse concernant la maladie de Mc Ardle :",
					questions: [
						"La maladie de Mc Ardle est une myopathie liée à une déficience en myophosphorylase",
						"Lors de l’exercice il y a une augmentation importante de l’acide lactique entrainant des contractures",
						"Elle se transmet sur un mode autosomal récessif",
						"Le phénomène du second souffle est lié à l’utilisation des lipides lors d’un exercice d’endurance chez ces sujets"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouvez la mauvaise réponse : les maladies suivantes sont des canalopathies",
					questions: [
						"La myotonie",
						"La paralysie périodique hypokaliémique",
						"La maladie de Duchenne",
						"La paramyotonie"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Trouver la bonne réponse : lors des enregistrements électroencéphalographiques pour exploration d’une épilepsie",
					questions: [
						"L’onde lente d’une activité pointe onde correspond à la somme des repolarisations des soma neuronaux par les courants liés au passage du sodium et les courants liés au passage du chlore au travers de la membrane",
						"Dans l’épilepsie partielle bénigne à pointes centro temporales les marqueurs EEG sont hérités de manière autosomale dominante",
						"Dans l’épilepsie partielle bénigne à pointes centro temporales les marqueurs EEG sont toujours associés à la survenue de crises d’épilepsie",
						"L’examen neurologique d’un jeune patient présentant une épilepsie de type absence avec mise en évidence d’activités de type pointe ondes généralisées à 3 Hz démontrera très fréquemment des anomalies en dehors des crises"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Trouver la bonne réponse : dans le syndrome de Guillain Barré (polyradiculonévrite inflammatoire démyélinisante aigüe):",
					questions: [
						"Le déficit moteur maximal est atteint le plus souvent après 8 semaines",
						"Le traitement par plasmaphérèse est moins efficace qu’un traitement par corticostéroïdes",
						"Le déficit moteur maximal est atteint le plus souvent avant la fin de la 4ème semaine",
						"Le déficit moteur maximal est atteint après 6 semaines"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle est la proposition vraie ? Le nystagmus d’origine vestibulaire :",
					questions: [
						"La direction de la phase rapide est vers le côté sain",
						"Augmente en amplitude lorsque le regard est dirigé vers le coté malade",
						"Est augmenté par la fixation",
						"Est associé à vertiges rotatoires"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "À propos du traitement de l’épilepsie, quelle est la proposition incorrecte ?",
					questions: [
						"Le traitement dépend du type de crise, de la comorbidité, du profil des effets secondaires, et des interactions pharmacologiques.",
						"On peut instaurer un traitement pharmacologique dès la première crise si le risque de récidive est élevé.",
						"La dose de maintenance du traitement anti épileptique n’est pas déterminée en premier lieu par le niveau sanguin du médicament, mais par la réponse clinique.",
						"Le traitement de première ligne consiste en une combinaison de médicaments avec des mécanismes d’action différents."
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "À propos du traitement de la maladie de Parkinson idiopathique à base de L-DOPA, quelle est la proposition correcte ?",
					questions: [
						"Il faut débuter le traitement avec une dose de charge suivie par une dose de maintien.",
						"Il faut la donner en une prise unique au réveil.",
						"Ses effets secondaires diminuent avec la durée de la maladie",
						"La L-DOPA donne moins d’hallucinations que les agonistes dopaminergiques"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient de 67 ans se présente avec des fasciculations, une légère amyotrophie et une faiblesse progressive des membres supérieur et inférieur droit, qui ont évolué depuis plusieurs mois. Les réflexes myotatiques sont vifs partout, les réponses cutanées plantaires sont en extension à gauche, les réflexes abdominaux sont absents. Il n’a pas de déficit sensitif ni de troubles sphinctériens. Parmi les suivants, quel est le diagnostic plus probable ?",
					questions: [
						"Sclérose latérale amyotrophique",
						"Syringomyélie",
						"Myopathie à inclusions",
						"Myélopathie sur canal cervical étroit"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Les AIT dans le territoire vertébro-basilaire peuvent causer des multiples symptômes. Parmi les suivants, quel symptôme n’est pas typique d’un AIT vertébro-basilaire :",
					questions: [
						"Syncope",
						"Vertiges et perte d’équilibre",
						"Hypoesthésie et paresthésies autour de la bouche",
						"Perte visuelle monoculaire (amaurose fugace)"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient de 76 ans, atteint d’une hémiparésie gauche séquellaire à un AVC sylvien droit, mais partiellement autonome et capable de marcher avec une canne, a fait une chute en absence de témoins. Il est amené à l’hôpital en urgence, où il arrive confus, somnolent et avec une augmentation de son hémiparésie gauche. Le CT en urgence montre une hypodensité séquellaire au premier AVC. Parmi les suivants, quelle est la cause plus probable de cet épisode :",
					questions: [
						"Une crise d’épilepsie secondaire au premier AVC",
						"Un nouvel AVC pas encore visualisé par le CT",
						"Un AIT",
						"Une arythmie cardiaque"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un patient arrive à la garde avec une diplopie à début aigu, associée à une douleur retro-oculaire droite. L’examen neurologique montre une ptose palpébrale et une déviation de l’œil droit vers l’extérieur et vers le bas, la pupille droite est mydriatique, les réflexes achilléens sont absents bilatéralement. Le patient est diabétique. Quelle est le diagnostic plus probable :",
					questions: [
						"Paralysie isolée du nerf III",
						"Myasthénie",
						"Syndrome de Guillain-Barré",
						"AVC du tronc cérébral"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Le nerf facial innerve les glandes suivantes à l’exception de :",
					questions: [
						"Sous-mandibulaire",
						"Lacrymale",
						"Parotide",
						"Palatine"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un homme de 50 ans arrive aux urgences en état confusionnel. Il a une histoire d’abus d’alcool, mais son taux alcoolique à l’admission est zéro. Il a des multiples altérations métaboliques, en particulier une hyperglycémie, une hyponatrémie, et une hypocalcémie, qui sont rapidement corrigées. Le patient d’abord s’améliore, mais après il développe une quadriparésie spastique et il devient encore plus confus. Quel est le diagnostic plus probable :",
					questions: [
						"Encéphalopathie de Wernicke",
						"AVC de la protubérance",
						"Myélinolyse pontique centrale",
						"Crise d’épilepsie sur sevrage alcoolique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un homme de 54 ans se présente avec une hémiparésie gauche qui est plus sévère au membre inférieur qu’au membre supérieur, en absence d’hémi-négligence et d’hémianopsie. Quel est le site plus probable d’occlusion vasculaire :",
					questions: [
						"L’artère cérébrale antérieure droite",
						"L’artère sylvienne droite",
						"La carotide interne droite",
						"L’artère vertébrale droite"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quelle est la cause principale d’AVC cardio-embolique chez les personnes âgées :",
					questions: [
						"Sténose aortique",
						"Fibrillation auriculaire",
						"Endocardite bactérienne subaigue",
						"Infarctus myocardique"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quel est le traitement plus indiqué pour la prévention secondaire d’un AVC cardioembolique dû à une fibrillation auriculaire :",
					questions: [
						"Anticoagulation",
						"Pacemaker cardiaque",
						"Antiagrégation plaquettaire par Aspirine + Clopidogrel",
						"Beta-bloquant pour réduire la fréquence cardiaque"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Un homme de 59 ans arrive aux urgences avec une hémiparésie gauche qui était présente au réveil. Il est obese, fumeur et hypertendu non traité. Il n’a pas de céphalée, pas de négligence, pas de déficit visuel. Son état est stable. La thrombolyse est :",
					questions: [
						"Exclue car l’heure d’installation du déficit n’est pas connue",
						"Indiqué si le CT de perfusion montre une zone de pénombre",
						"Exclue car le patient est hypertendu non traité",
						"Indiqué car le déficit n’est pas trop sévère suggérant une zone de pénombre"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Une femme de 24 ans, deux semaines après avoir accouché, développe une céphalée accompagnée par nausée et vomissements et fait une crise d’épilepsie tonico-clonique. À l’arrivée aux urgences, elle est en léger état confusionnel et un œdème papillaire est observé à l’examen du fond d’œil. Quel est le diagnostic plus probable :",
					questions: [
						"AVC cardioembolique",
						"Hypertension intracrânienne idiopathique",
						"Méningite bactérienne",
						"Thrombose de sinus veineux"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Une femme de 51 ans, hypertendue non traitée, arrive à la garde en état de coma, perte des réflexes du tronc cérébral et posture en décérébration. L’installation des symptômes a été brutale. Quelle est la cause plus probable :",
					questions: [
						"Etat de mal épileptique",
						"Encéphalopathie hypertensive",
						"Hémorragie dans la protubérance",
						"Hémorragie sous-arachnoïdienne"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Une femme de 58 ans se plaint d’un tremblement des mains qui a évolué lentement depuis plusieurs années et qui la gêne surtout quand elle tient un verre pour boire, quand elle écrit (son écriture est devenue grande et irrégulière) et quand elle fait des manipulations précises. Le tremblement est augmenté par l’anxiété. Elle a observé que boire un verre de vin soulage son tremblement. Son père tremblait aussi. Quel est le diagnostic plus probable :",
					questions: [
						"Maladie de Parkinson familiale",
						"Tremblement essentiel",
						"Maladie de Huntington",
						"Tremblement psychogène dû à l’anxiété"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Dans quelle méningite virale on trouve assez souvent une diminution du glucose dans le LCR :",
					questions: [
						"HSV",
						"Oreillons",
						"Rougeole",
						"Rubéole"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un homme de 27 ans arrive aux urgences avec fièvre, céphalée, confusion, difficulté à s’exprimer, déficit de mémoire, évoluant depuis 24 heures. Il fait une crise d’épilepsie. Un CT scan sans contraste est normal. La ponction lombaire montre une augmentation des protéines, glucose normal, 150 GB/μl et 130 GR/μl. Quelle est la meilleure approche à suivre :",
					questions: [
						"Le diagnostic plus probable est une encéphalite à arbovirus. Il faut faire une IRM et donner un traitement de soutien en attendant les résultats des cultures et PCR.",
						"Le diagnostic plus probable est une encéphalite autoimmune. Il faut faire une IRM et donner un traitement de soutien et de corticoïdes à forte dose en attendant les résultats des cultures et PCR.",
						"Le diagnostic plus probable est une encéphalite herpétique. Il faut commencer le traitement par acyclovir en attendant les résultats des autres examens complémentaires, y compris cultures et PCR.",
						"Le diagnostic plus probable est une encéphalite herpétique. Il faut faire une IRM car le CT ne montre pas d’anomalie des lobes temporaux."
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Un homme de 29 ans atteint de SIDA présente depuis 1 an des progressives difficultés de concentration, déficit attentionnel, perte de mémoire, ralentissement psychomoteur, perte de coordination des mouvements. Il est devenu dépendant pour ses activités de la vie journalière. L’IRM montre une légère atrophie diffuse sans lésion focale. L’analyse du LCR montre une légère augmentation des protéines et des cellules avec glucose normal. Quel est le diagnostic plus probable parmi les suivants :",
					questions: [
						"Leukoencéphalopathie multifocale progressive (virus JC)",
						"Lymphome primaire du SNC",
						"Méningite à cryptocoque",
						"Démence à HIV"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quelle est la proposition fausse à propos de la maladie de Huntington :",
					questions: [
						"Rigidité et bradykinésie peuvent être présentes surtout si le début des symptômes est précoce",
						"Un déficit isolé de mémoire est typique de la phase initiale de maladie",
						"Le suicide est fréquent",
						"Dans la même famille, l’âge de début des symptômes peut être variable"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Parmi les suivantes, quelle est une forme irréversible de démence :",
					questions: [
						"Hydrocéphalie normotensive",
						"Hématome sous-dural",
						"Démence fronto-temporale",
						"Déficit de vitamine B12"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle est la proposition fausse à propos des paraplégies spastiques héréditaires :",
					questions: [
						"Une ataxie cérébelleuse peut être présente dans certaines formes",
						"Il s’agit toujours de maladies autosomiques dominantes",
						"Des dizaines de gènes différents sont impliqués",
						"Une polyneuropathie périphérique peut être présente dans certaines formes"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Parmi les suivantes, quelle caractéristique n’est retrouvé dans l’ataxie-télangiectasie :",
					questions: [
						"Élévation de l’alpha fœto-protéine",
						"Prédisposition aux cancers",
						"Hérédité liée au chromosome X",
						"Mouvements involontaires de type choréique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Une patiente de 58 ans se présente avec un tremblement de la main droite, une douleur à l’épaule droite, constipation et agitation nocturne. L’examen neurologique montre une rigidité en roue dentée des membres de droite et un ralentissement des mouvements fins du même côté. Le tremblement disparaît au maintien de la posture et durant le mouvement, il augmente lorsque la patiente est concentrée sur une tache motrice avec la main controlatérale. Quel est le diagnostic plus probable :",
					questions: [
						"Cervico-brachialgie droite",
						"Tremblement essentiel",
						"Maladie de Parkinson",
						"Dépression avec somatisation"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Quelle est la proposition fausse ? La sclérose en plaques:",
					questions: [
						"Cause une atrophie cérébrale progressive",
						"Les lymphocytes B ne sont pas impliqués dans la pathogénie",
						"Peut donner des lésions dans la substance grise",
						"Est caractérisée par la présence de lymphocytes T activés dans le parenchyme cérébral"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Quelle est la proposition correcte ? L’atteinte des cordons postérieurs de la moelle épinière cause :",
					questions: [
						"Une augmentation des réflexes myotactiques",
						"Une amyotrophie",
						"Une perte de sensibilité thermique",
						"Une ataxie proprioceptive"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quelle est la proposition fausse ? Dans la sclérose en plaques (SeP) :",
					questions: [
						"Les poussées sont plus rares durant la grossesse",
						"La forme secondairement progressive est la transformation d’une forme à poussées-remissions après plusieurs années de maladie",
						"Les traitements immunomodulateurs sont efficaces si le patient présente des poussées",
						"La forme progressive primaire est la forme clinique de présentation dans ~50% des cas"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Quelle est la proposition fausse ? Les crises de migraine sont caractérisées par :",
					questions: [
						"Photophobie",
						"Douleur toujours unilatérale",
						"Douleur plus souvent pulsatile",
						"Nausée"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un homme de 56 a développé sur plusieurs mois une lenteur des mouvements et une raideur musculaire associées à épisodes d’hypotension orthostatique et à un trouble érectile. L’IRM montre une réduction de volume des noyaux de la base, mais pas de lésion focale. Un essai thérapeutique de deux mois avec de la L-DOPA a donné un bénéfice très limité. À ce stade, quel est le diagnostic plus probable ?",
					questions: [
						"Maladie de Parkinson",
						"Maladie de Wilson",
						"Atrophie multi-systémique (MSA)",
						"Maladie à corps de Lewy diffus"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. À quoi pensez-vous ?",
					questions: [
						"à une gastro-entérite",
						"à une infection virale",
						"à une tumeur cérébrale",
						"à une hémorragie cérébrale"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quelle mise au point effectuez-vous ?",
					questions: [
						"un EEG",
						"un fond d’œil",
						"une imagerie médicale",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quel traitement proposez-vous ?",
					questions: [
						"une chimiothérapie",
						"une chirurgie",
						"une radiothérapie",
						"aucune réponse n’est correcte"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Quelle est la tumeur la plus fréquente chez l’enfant ?",
					questions: [
						"un astrocytome pilocytique",
						"un glioblastome",
						"une tumeur embryonnaire ",
						"réponses a. et c. correctes"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Enfant de 9 ans, présentant des nausées, des vomissements et une perte de poids depuis 3 semaines. Il est apathique. La tête en est rotation vers la gauche. Dans les médulloblastomes chez les enfants, le pronostic dépend la plus fréquente chez l’enfant ?",
					questions: [
						"de la taille de la résection",
						"des anomalies génétiques",
						"de l’âge",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. Quelle est son échelle de Glasgow ?",
					questions: [
						"4",
						"4T",
						"5T",
						"5"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. Quelle prise en charge préconisez-vous ?",
					questions: [
						"la mise en place d’une PIC",
						"une ponction lombaire",
						"l’administration de mannitol",
						"une surveillance"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title:"Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. La ponction lombaire ne peut se faire que ?",
					questions: [
						"lorsqu’une imagerie a exclu un processus expansif",
						"au niveau L4-L5",
						"au niveau L5-S1",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. La mesure de pression intracrânienne est indiquée dans les traumatismes crâniens lorsque :",
					questions: [
						"on note des signes d’hypertension intracrânienne",
						"le Glasgow est inférieur à 7",
						"le scanner est anormal",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Un patient intubé arrive aux urgences suite à un accident sur la voie publique. L’examen neurologique met en évidence une hémi-parésie avec des signes de décérébration, une ouverture des yeux à la douleur, des pupilles réflectiques et un réflexe fronto-orbitaire absent. Dans les traumatismes crâniens, l’hypercapnie peut entrainer :",
					questions: [
						"une vasodilatation",
						"l’augmentation de la PAO2",
						"une diminution du diamètre des artères",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. Les ondes de la mesure de PIC fluctuent en fonction :",
					questions: [
						"de la respiration",
						"du rythme cardiaque",
						"de la toux",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. La pression est supérieure à 20 mm Hg. Que proposez-vous ?",
					questions: [
						"le drainage du liquide céphalorachidien",
						"le contrôle de la tension artérielle",
						"de mettre la tête du patient à 30°",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. Quelle(s) est (sont) la (les) complication(s) possible(s) chez ces patients :",
					questions: [
						"un vasospasme",
						"une hydrocéphalie",
						"une ventriculite (infection des ventricules)",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "Aux Soins Intensifs, un patient comateux est intubé et ventilé suite à une hémorragie sous-arachnoïdienne ; une mesure de pression intracrânienne est en place. L’hémorragie sous-arachnoïdienne peut avoir comme origine la plus probable :",
					questions: [
						"un traumatisme crânien",
						"la rupture d’un anévrisme",
						"la rupture d’une malformation artério-veineuse",
						"toutes les réponses sont correctes"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Quel est le 1er examen à réaliser ?",
					questions: [
						"une IRM cérébrale",
						"un CT-Scanner",
						"une ponction lombaire",
						"une angiographie cérébrale"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Quel est le grade clinique WFNS de la patiente :",
					questions: [
						"WFNS 1",
						"WFNS 2",
						"WFNS 3",
						"WFNS 4"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				},
				{
					title: "Patiente de 58 ans, sans antécédent personnel et familial hormis une consommation tabagique. Céphalée brutale, qu’elle décrit comme la pire de sa vie. Examen clinique : raideur de nuque, photophobie, hémiparésie droite. Un anévrisme est diagnostiqué. On sait que :",
					questions: [
						"les anévrismes les plus fréquents sont situés en circulation postérieure",
						"les anévrismes de l’artère carotide interne sont très rares",
						"les anévrismes sont le plus souvent situés à distance du polygone de Willis",
						"les anévrismes de l’artère communicante antérieure sont les plus fréquents"
					],
					answer: null,
					rightAnswer: 4,
					result: null
				},
				{
					title: "La myélinisation",
					questions: [
						"est un phénomène dynamique qui ne se fait pas au même moment pour toutes les structures du cerveau",
						"commence d’abord dans les lobes temporaux",
						"s’arrête à l’adolescence",
						"commence d’abord dans les lobes occipitaux"
					],
					answer: null,
					rightAnswer: 1,
					result: null
				},
				{
					title: "Quelle est la présentation clinique d’un enfant âgé de 2 ans après un AVC sylvien congénital gauche",
					questions: [
						"Aphasie",
						"Hémiplégie droite + aphasie",
						"Hémiplégie droite",
						"Diplégie spastique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La myopathie de Duchenne",
					questions: [
						"est visible cliniquement dès la naissance",
						"se manifeste par un tableau relativement stable",
						"s’accompagne d’une élévation des CK",
						"Diplégie spastique"
					],
					answer: null,
					rightAnswer: 3,
					result: null
				},
				{
					title: "La grande prématurité",
					questions: [
						"s’accompagne de troubles cognitifs et comportementaux dans 5 à 10% des cas",
						"s’accompagne de troubles cognitifs et comportementaux dans 25 à 50% des cas",
						"s’accompagne dans plus de 15% des cas d’une infirmité motrice cérébrale de type diplégie spastique",
						"concerne les enfants nés entre 32 et 37 semaines d’âge gestationnel"
					],
					answer: null,
					rightAnswer: 2,
					result: null
				}
			]
		}
	},
	methods:{
		checkAnswer(i){
			this.quiz[i].result = this.quiz[i].answer === this.quiz[i].rightAnswer;
			this.$refs.stepper.next();
		},
		showResult(){
			this.loading = !this.loading;
			clearInterval(this.interval);

			this.resultArr = this.quiz.filter(el => !el.result);
			this.test = !this.test;

			this.loading = !this.loading;
		},
		timer(){
			this.interval = setInterval(()=>{
				if(this.seconds === 0){
					this.seconds = 59;
					this.minutes -= 1;
				}
				this.seconds -=1;
				if(this.seconds === 0 && this.minutes === 0){
					clearInterval(this.interval);
					this.resultArr = this.quiz.filter(el => !el.result);
					this.test = !this.test;
				}
			}, 1000)
		}
	},
	created() {
		let bucket;

		for(let i = 0; i < this.quiz.length; i++){
			let randomNum = Math.floor(Math.random() * this.quiz.length);

			bucket = this.quiz[i];
			this.quiz[i] = this.quiz[randomNum];
			this.quiz[randomNum] = bucket;
		}
	},
	mounted(){
		this.timer();
	}
}
</script>
<style>
	.styling__radio{
		background: lightgray;
	}

	.q-stepper__title{
		font-family: 'Ubuntu', sans-serif;
	}
</style>
