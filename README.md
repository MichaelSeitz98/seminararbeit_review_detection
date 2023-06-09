# Erzeugung und Detektion multimodaler Deepfakes bei Online Rezensionen

Dieses Projekt beschäftigt sich mit der Erkennung von gefälschten Bewertungen (Fake-Reviews) auf Online-Plattformen. Hierbei wird ein multimodaler Ansatz verfolgt, der auf den verschiedenen Datenmodalitäten Text, Bild und tabellarische Metadaten basiert.

## Architektur :construction::bulb:


<img src="02_Images/graphics/artefact_uebersicht_v5.jpg" alt="it-artefakt" width="710">


## Wichtige Notebooks:notebook:

1. `nb_generate_dataset.ipynb`: In diesem Notebook werden relevante Informationen aus den Daten extrahiert, verarbeiten, Daten herausgefiltert. Als Resultat wird der  Basissätze `real_base_sent_reduced.csv` generiert. 

2. `nb_generate_deepfakes.ipynb`: Auf Basis der Kategorie aus Notebook 1 werden hier auf allen Modalitäten Fake-Reviews generiert. Hier werden der echte Datensatz `base_....csv` und der gefälschte Datensatz `fake_base_gpt_3_...` zusammengeführt zu `merged_fake_real.csv`. Dieser Datensatz dient als neue Ausgangsbasis für die Detektion.

3. `nb_feature_extraction.ipynb`: Hier werden die Features für alle Modalitäten extrahiert und im Datensatz `features_enrichted[..].csv` gespeichert. Dieser Datensatz wird für die Detektion herangezogen.

4. `nb_fake_detection.ipnyb`: Zuerst wird der Datensatz in Trainings-, Validierungs- und Testdatensatz aufgeteilt (`detection_train.csv`, `detection_val.csv` und `detection_test.csv`). Anschließend werden XGBoost, Random Forest und TabNet trainiert und die Hyperparameter anhand des Validierungsdatensatzes optimiert. Abschließend wird das Modell evaluiert.

5. `nb_generated_gpt3_finetune_datasets.ipynb`: In diesem Notebook werden verschiedene GPT3 Finetunes vorbereitet und dafür benötigte Datensätze erzeugt.

6. `nb_frontend_showcase.ipynb`: Hier für die Präsentation anhand einer prototypischen Frontend das Artefakt vorgestellt. Durch Auführen aller Zellen, kann die Frontendanwendung gestartet werden. Zum einen besteht diese aus einem Quiz: "Fake oder Echt", worin man testen kann ob man selbst erkennt ob es sich um eine Deepfake oder um eine echte Review handelt. Der zweite Teil der Frontend-Anwendung "Generierung mulitmodaler Deepfakes" verdeutlicht wie einfach multimodale Deepfakes mittels generativer KI erstellt werden können. Dies verdeutlich die (zukünftige) Relevanz von multimodalen Detektionsmethoden.

## Wichtige Datensätze :file_folder:

Die bedeutenden Datensätze sind im Order abgelegt [01_Data/data_cleaned_processed/](01_Data/data_cleaned_processed/) abgelegt. Die rohe (und leicht vorverarbeitete) Datensätz unter [01_Data/raw_data/](01_Data/raw_Data/). Für die Finetunes verwendeten Datensätze sind in [01_Data/gpt3_finetunes](01_Data/gpt3_finetunes/) abgelegt. 

1.  `real_base_sent_reduced.csv`: enthält Informationen von 704 echten multimodalen (Text, Bild, Metainformationen) echten Google Maps Reviews, zufällig ausgewählt aus über ca. 9000 multimodalen Reviews von  `l.01_Data\raw_data\dataset_weitere_forschung_relCols.csv` Der Datensatz wurde schon auf wesentliche Spalten reduziert und wird im Notebook `nb_generate_dataset.ipynb` generiert.

2. `fake_gpt3_all_finetunes_sent__dalle_tab.csv`: bildet die Fake-Reviews ab, alle Modalitäten mit unterschiedlichen Technologien erzeugt und als Spalte angehängt (GPT3 Finetune, DALL-E-2, CTGAN und Faker genutzt). Wird in  Notebook  `nb_generate_deepfakes.ipynb` erzeugt. Darüber lässt sich auch auf die künstliche genierten Bilder zugreifen. Hier sind auch die Ergebnisse der verschiendeenen Finetune-Varianten enthalten. 

3.  `base_for_feature_extraction.csv`: bildet den **zusammengesetzten Datensatz** aus 704 multimodalen Fakes und 704 Echten Reviews ab, mit URL zu den echten und gefakten Bildern. Ist Grundlage für Feature Extraction und Detektion. Darüber lässt sich auch auf die künstliche genierten Bilder zugreifen. Als Text wird dazu die Finetune-Variante FT v4 verwendet.

4. `features_enriched_tab_img_text.csv`: ist mit den extrahierten Features aller Modalitäten angereichert. Wird im Notebook `nb_feature_extraction.ipynb` erstellt. Nach einigen kleineren preprocessing Schritten nochmals in Form von `features_enriched_tab_img_text_preproc.csv` abgespeichert. Bildet die Basis für den Data Split bzw. die Detektion.

5. Datensätze für den Data Split:
- `detection_train.csv`
- `detection_test.csv`
- `detection_val.csv`

Es ist wichtig, dass alle Modelle und alle Modalitäten immer auf den gleichen Daten trainiert, getuned und evaluiert werden. Deshalb werden Datensätze mit 80:20 Split Train_Val:Test und nochmals 80:20 Split Train:Val erzeugt. Zu Beginn von Notebook `nb_fake_detection.ipnyb` werden diese abgespeichert.

6.  `results_image_classifier.csv`: Dort ist die separate Auswertung des alternativen "abgeschlossenen Bildklassifikator-Ansatzes" ist unter aus dem Notebook `nb_generated_gpt3_finetune_datasets.ipynb` abgespeichert

## Weitere Informationen zur Handhabung :question:

- Die künstlich generierten Bilder von DALL-E-2 können über die Datensätze `base_for_feature_extraction.csv` sowie `fake_gpt3_all_finetunes_sent__dalle_tab.csv` abgerufen werden. Um das zu ermöglichen sind die Bilder im Ordner [02_Images/](02_Images/) gehostet.
- Die rohe Daten mit den unverarbeiteten Google Maps Reviews finden sich im Ordner [01_Data/raw_data](01_Data/raw_data/). Je nach Kategorie (Restaurant,Hotel und  Aktivität) findet sich auch eine Auflistung alle potenziell verfügbaren Spalten, z.B. [Hier](01_Data/raw_data/cols_activities.txt) alle Spalten der gescrapten Aktivitäten.
- Es wurden mehrere Varianten von GPT3 Finetunes entwickelt und auf den Datensatz angewendet. Die hierfür benötigte Datensätze (inkl. Prompt und Prompt-Completions) sind unter [01_Data/finetuning/](01_Data/finetuning/) abgespeichert.
- Die SHAP Graphiken sind im Ordner [02_Images/graphics](02_Images/graphics) abgelegt, 
- Visualisierungen des Bild-Features (durch Aktive Maximierung-Technologie) in [02_Images/visualizations_image_features](02_Images/visualizations_image_features) 
- Einige nicht direkt verwendete Dateien, die evtl. für zukünftige Untersuchungen relevant bleiben könnten, sind im Ordner [09_outdated_archived_or_rejected_approaches](09_outdated_archived_or_rejected_approaches/) abgelegt. Darunter fallen auch Ansätze mit anderen Technologien (Stable Diffusion, GPT-2, GPT-3.5 Turbo, AutoTrain), die letztendlich nicht verwendet wurden. Außerdem ist dort ein gescrapter [Datensatz multimodaler Amazon-Reviews](09_outdated_archived_or_rejected_approaches/amazon/dataset_amazon_reviews_v1.csv) abgelegt. Ergebnisse von Zwischenschritte sind teilweise ebenfalls dort abgelegt. 
- [comparison_finetune_variations_50sample.xlsx](01_Data\data_cleaned_processed\comparison_finetune_variations_50sample.xlsx) stellt einen übersichtlich für den Vergleich der Finetunes FT2-4dar (Anmerkung: FT3 in zwei Versionen, 3.2 auf Basis von 3.1 weitertrainiert)
  

## Ergebnisse :bar_chart:

Die Ergebnisse der vergleichenden Untersuchung in Abhängigkeit der Modalitäten und der drei verwendeten ML-Methoden XGBoost, Random Forest und TabNet:

<!-- ![heatmap](02_Images/graphics/heatmap_results.png) -->
<img src="02_Images/graphics/heatmap_results.png" alt="heatmap" width="500">



<!-- | Modalität(en)      | Model    | Accuracy | Precision | Recall | F1     | AUC    |
|--------------------|----------|----------|-----------|--------|--------|--------|
| Unabhängig       | Coinflip | 0.4964   | 0.4966    | 0.5177 | 0.5069 | 0.4964   |
| Text               | TabNet   | 0.5248   | 0.5235    | 0.5532 | 0.5379 | 0.5248 |
| Text               | RF       | 0.5142   | 0.5147    | 0.4965 | 0.5054 | 0.5142 |
| Text               | XGB      | 0.5567   | 0.5548    | 0.5745 | 0.5645 | 0.5567 |
| Img                | TabNet   | 0.8901   | 0.8929    | 0.8865 | 0.8897 | 0.8901 |
| Img                | RF       | 0.8901   | 0.8986    | 0.8794 | 0.8889 | 0.8901 |
| Img                | XGB      | 0.8865   | 0.8811    | 0.8936 | 0.8873 | 0.8865 |
| Tab                | TabNet   | 0.6702   | 0.7727    | 0.4823 | 0.5939 | 0.6702 |
| Tab                | RF       | 0.7305   | 0.7778    | 0.6454 | 0.7054 | 0.7305 |
| Tab                | XGB      | 0.7199   | 0.7385    | 0.6809 | 0.7085 | 0.7199 |
| Tab+Text           | TabNet   | 0.6064   | 0.6339    | 0.5035 | 0.5613 | 0.6064 |
| Tab+Text           | RF       | 0.7199   | 0.7627    | 0.6383 | 0.6950 | 0.7199 |
| Tab+Text           | XGB      | 0.7163   | 0.7163    | 0.7163 | 0.7163 | 0.7163 |
| Tab+Img            | TabNet   | 0.9184   | 0.9214    | 0.9149 | 0.9181 | 0.9184 |
| Tab+Img            | RF       | 0.8936   | 0.9051    | 0.8794 | 0.8921 | 0.8936 |
| Tab+Img            | XGB      | 0.9291   | 0.9172    | 0.9433 | 0.9301 | 0.9291 |
| Text+Img | TabNet | 0.9078 | 0.9323 | 0.8794 | 0.9051 | 0.9078 |
| Text+Img | RF | 0.8830 | 0.8913 | 0.8723 | 0.8817 | 0.8830 |
| Text+Img | XGB | 0.8972 | 0.8944 | 0.9007 | 0.8975 | 0.8972 |
|Tab+Text+Img | TabNet | 0.9326 | 0.9178 | 0.9504 | 0.9338 | 0.9326 |
| Tab+Text+Img | RF | 0.8901 | 0.8986 | 0.8794 | 0.8889 | 0.8901 |
| Tab+Text+Img | XGB | 0.9149 | 0.9149 | 0.9149 | 0.9149 | 0.9149 | -->

## Demo-Anwendung :man_technologist:

Zum Starten der Demoanwendung: Alle Zellen des Notebooks `nb_frontend_showcase.ipynb` ausführen. Für den "Ein-Klick-Deepfake-Generator" noch den eigenen OpenAI-Key reinladen.

##### Ein-Klick-Deepfake-Generator

https://github.com/MichaelSeitz98/seminararbeit_review_detection/assets/65539604/05069901-5ee3-491e-bd89-1f643bbe6dab

Demo-Anwendung zeigt **nicht** das finegetunte Modell, welches tatsächlich für die Textgenerieung des Datensatzes verwendet wurde, sondern ein abgewandelter, weniger trainierter Finetune davon.

##### Echt-oder-Fake-Quiz

https://github.com/MichaelSeitz98/seminararbeit_review_detection/assets/65539604/703b5b96-324c-422c-ad0c-f98f22c1a5af



## Erklärbare Vorhersagen mit SHAP-Analyse :bulb:

Für eine detaillierte Analyse wurden die Features für jede Modalität und Modalitätskombination mittels SHAP am Beispiel des XGBoost-Verfahren analysiert. Einige Plots sind hier aufgelistet:

##### Text
<img src="02_Images/graphics/shap_xgb_text.png" alt="Text Modalität" width="550">


##### Tabellarisch
<img src="02_Images/graphics/shap_xgb_tab.png" alt="Tab Modalität" width="550">


##### Bild
<img src="02_Images/graphics/shap_xgb_img0_10.png" alt="Img Modalität" width="550">


##### Kombination aus Tab + Text

<img src="02_Images/graphics/shap_xgb_tab_text.png" alt="Tab + Text Modalität" width="550">


##### Kombination aus Tab + Img

<img src="02_Images/graphics/shap_xgb_tab_img0.png" alt="Tab + Img Modalität" width="550">



##### Alle Modalitäten

<img src="02_Images/graphics/shap_xgb_tab_text_img0.png" alt="Tab + Text + Img Modalität" width="550">
