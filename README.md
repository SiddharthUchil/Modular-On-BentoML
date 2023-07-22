<div align="center">
    <h1 align="center">Transformer Models as a Service</h1>
    <br>
    <strong>A modular, composable, and scalable solution for building NLP services with Transformers<br></strong>
    <i>Powered by BentoML 🍱 + HuggingFace 🤗</i>
    <br>
</div>
<br>

## 📖 Introduction 📖
- This project showcase how one can serve HuggingFace's transformers models for various NLP with ease.
- It incorporates BentoML's best practices, from setting up model services and handling pre/post-processing to deployment in production.
- User can explore the example endpoints such as summarization and categorization via an interactive Swagger UI.
  
![Transformers-NLP-Service](https://github.com/SiddharthUchil/Transformers-NLP-Service/assets/36127139/817f865b-9f9c-452b-9e9b-ac70aa17d061)

## 🏃‍♂️ Running the Service 🏃‍♂️
To fully take advantage of this repo, we recommend you to clone it and try out the service locally. 

### BentoML CLI
This requires Python3.8+ and `pip` installed.

```bash
git clone https://github.com/bentoml/transformers-nlp-service.git && cd transformers-nlp-service

pip install -r requirements/tests.txt

bentoml serve
```

You can then open your browser at http://127.0.0.1:3000 and interact with the service through Swagger UI.

### Containers

There are two pre-built container to run on CPU and GPU respectively. 

```bash
# cpu
docker run -p 3000:3000 ghcr.io/bentoml/transformers-nlp-service:cpu

# gpu
docker run --gpus all -p 3000:3000 ghcr.io/bentoml/transformers-nlp-service:gpu
```

> Note that to run with GPU, you will need to have [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) setup.

### Python API
One can also use the BentoML Python API to serve their models.

Run the following to build a Bento within the Bento Store:
```bash
bentoml build
```
Then, start a server with `bentoml.HTTPServer`:

```python
import bentoml

# Retrieve Bento from Bento Store
bento = bentoml.get("transformers-nlp-service")

server = bentoml.HTTPServer(bento, port=3000)
server.start(blocking=True)
```

### gRPC?
If you wish to use gRPC, this project also include gRPC support. Run the following:

```bash
bentoml serve-grpc
```

To run the container with gRPC, do

```bash
docker run -p 3000:3000 -p 3001:3001 ghcr.io/bentoml/nlp:cpu serve-grpc
``` 


## 🌐 Interacting with the Service 🌐
The default mode of BentoML's model serving is via HTTP server. Here, we showcase a few examples of how one can interact with the service:
### cURL
The following example shows how to send a request to the service to summarize a text via cURL:

```bash
curl -X 'POST' \
  'http://0.0.0.0:3000/summarize' \
  -H 'accept: text/plain' \
  -H 'Content-Type: text/plain' \
  -d 'The three words that best describe Hunter Schafer'\''s Vanity Fair Oscars party look? Less is more.
Dressed in a bias-cut white silk skirt, a single ivory-colored feather and — crucially — nothing else, Schafer was bound to raise a few eyebrows. Google searches for the actor and model skyrocketed on Sunday night as her look hit social media. On Twitter, pictures of Schafer immediately received tens of thousands of likes, while her own Instagram post has now been liked more than 2 million times.
Look of the Week: Zendaya steals the show at Louis Vuitton in head-to-toe tiger print
But more than just creating a headline-grabbing moment, Schafer'\''s ensemble was clearly considered. Fresh off the Fall-Winter 2023 runway, the look debuted earlier this month at fashion house Ann Demeulemeester'\''s show in Paris. It was designed by Ludovic de Saint Sernin, the label'\''s creative director since December.
Celebrity fashion works best when there'\''s a story behind a look. For example, the plausible Edie Sedgwick reference in Kendall Jenner'\''s Bottega Veneta tights, or Paul Mescal winking at traditional masculinity in a plain white tank top.
For his first Ann Demeulemeester collection, De Saint Sernin was inspired by "fashion-making as an authentic act of self-involvement." It was a love letter — almost literally — to the Belgian label'\''s founder, with imagery of "authorship and autobiography" baked into the clothes (Sernin called his feather bandeaus "quills" in the show notes).
Hunter Schafer'\''s barely-there Oscars after party look was more poetic than it first seemed.
These ideas of self-expression, self-love and self-definition took on new meaning when worn by Schafer. As a trans woman whose ascent to fame was inextricably linked to her gender identity — her big break was playing trans teenager Jules in HBO'\''s "Euphoria" — Schafer'\''s body is subjected to constant scrutiny online. The comment sections on her Instagram posts often descend into open forums, where users feel entitled (and seemingly compelled) to ask intimate questions about the trans experience or challenge Schafer'\''s womanhood.
Fittingly, there is a long lineage of gender-defying sentiments stitched into Schafer'\''s outfit. Founded in 1985 by Ann Demeulemeester and her husband Patrick Robyn, the brand boasts a long legacy of gender-non-conforming fashion.
"I was interested in the tension between masculine and feminine, but also the tension between masculine and feminine within one person," Demeulemeester told Vogue ahead of a retrospective exhibition of her work in Florence, Italy, last year. "That is what makes every person really interesting to me because everybody is unique."
In his latest co-ed collection, De Saint Sernin — who is renowned in the industry for his eponymous, gender-fluid label — brought his androgynous world view to Ann Demeulemeester with fitted, romantic menswear silhouettes and sensual fabrics for all (think skin-tight mesh tops, leather, and open shirts made from a translucent organza material).
Celebrity stylist Law Roach on dressing Zendaya and '\''faking it '\''till you make it'\''
A quill strapped across her chest, Schafer let us know she is still writing her narrative — and defining herself on her own terms. There'\''s an entire story contained in those two garments. As De Saint Sernin said in the show notes: "Thirty-six looks, each one a heartfelt sentence."
The powerful ensemble may become one of Law Roach'\''s last celebrity styling credits. Roach announced over social media on Tuesday that he would be retiring from the industry after 14 years of creating conversation-driving looks for the likes of Zendaya, Bella Hadid, Anya Taylor-Joy, Ariana Grande and Megan Thee Stallion.'
```

## ⚙️ Customization ⚙️
### What if I want to add tasks *X*?

This project is designed to be used with different [NLP tasks](https://huggingface.co/tasks) and its corresponding models:

| Tasks                                                                               	| Example model                                                                                                               	|
|-------------------------------------------------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------	|
| [Conversational](https://huggingface.co/tasks/conversational)                     	| [`facebook/blenderbot-400M-distill`](https://huggingface.co/facebook/blenderbot-400M-distill)                               	|
| [Fill-Mask](https://huggingface.co/tasks/fill-mask)                               	| [`distilroberta-base`](https://huggingface.co/distilroberta-base)                                                           	|
| [Question Answering](https://huggingface.co/tasks/question-answering)             	| [`deepset/roberta-base-squad2`](https://huggingface.co/deepset/roberta-base-squad2)                                         	|
| [Sentence Similarity](https://huggingface.co/tasks/sentence-similarity)           	| [`sentence-transformers/all-MiniLM-L6-v2`](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)                   	|
| [Summarisation](https://huggingface.co/tasks/summarization)                       	| [`sshleifer/distilbart-cnn-12-6`](https://huggingface.co/sshleifer/distilbart-cnn-12-6) [included]                          	|
| [Table Question Answering](https://huggingface.co/tasks/table-question-answering) 	| [`google/tapas-base-finetuned-wtq`](https://huggingface.co/google/tapas-base-finetuned-wtq)                                 	|
| [Text Classification](https://huggingface.co/tasks/text-classification)           	| [`distilbert-base-uncased-finetuned-sst-2-english`](https://huggingface.co/distilbert-base-uncased-finetuned-sst-2-english) 	|
| [Text Generation](https://huggingface.co/tasks/text-generation)                   	| [`bigscience/T0pp`](https://huggingface.co/bigscience/T0pp)                                                                 	|
| [Token Classification](https://huggingface.co/tasks/token-classification)         	| [`dslim/bert-base-NER`](https://huggingface.co/dslim/bert-base-NER)                                                         	|
| [Zero-Shot Classification](https://huggingface.co/tasks/zero-shot-classification) 	| [`facebook/bart-large-mnli`](https://huggingface.co/facebook/bart-large-mnli) [included]                                    	|
| [Translation](https://huggingface.co/tasks/translation)                           	| [`Helsinki-NLP/opus-mt-en-fr`](https://huggingface.co/Helsinki-NLP/opus-mt-en-fr)                                           	|

### Where can I add models?
You can add more tasks and models by editing the `download_model.py` file.

### Where can I add API logics?
Pre/post processing logics can be set in the `service.py` file.


### Where can I find more docs about Transformers and BentoML?
BentoML supports Transformers models out of the box. You can find more details in the [BentoML support](https://docs.bentoml.org/en/latest/frameworks/transformers.html) for [Transformers](https://huggingface.co/docs/transformers/index).

