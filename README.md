[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [<img src="https://img.shields.io/badge/View-Website-blue">](https://coffee-chain-deploy-cfc.mybluemix.net/) [<img src="https://img.shields.io/badge/View-Video-red">](https://youtu.be/YXCFy-EfQ7s)


# Coffee Chain

<img src="https://i.ibb.co/pfGfzkL/logos.png" width=300> 

#### Marketplace con sistemas de pago digital, que brinda trazabilidad y certificados digitales basados en Blockchain.

WebPage: https://coffee-chain-deploy-cfc.mybluemix.net/

#### Escanee un Producto con este QR:

<img src="./Images/product.jpeg">

#### Click para ver nuestro demo video:

[<img src="https://raw.githubusercontent.com/altaga/SCUP-WWAC/master/Images/click-here-button.png" width=200>](https://youtu.be/YXCFy-EfQ7s)



<details>
  <summary>INDEX Click Here</summary>

- [CoffeeChain-CFC](#coffeechain-cfc)
- [Welcome to Coffee Chain:](#welcome-to-coffee-chain)
- [Problem:](#problem)
- [Solution:](#solution)
  - [DEMO:](#demo)
  - [How it Works:](#how-it-works)
  - [Cloudant:](#cloudant)
  - [API and Actions:](#api-and-actions)
  - [Chatbot:](#chatbot)
  - [Toolchain (CI/CD):](#toolchain-cicd)
  - [Solana Blockchain Integration:](#solana-blockchain-integration)
  - [Rapyd Integration:](#rapyd-integration)
- [Deployment/Traction:](#deploymenttraction)

</details>

# Bienvenidos a Coffee Chain:

Es una plataforma donde puedes comprar productos auténticos, socialmente responsables y 100% orgánicos, validados y rastreados por blockchain.

Repasemos el problema, 

México, entre una población de 127 millones, tiene 28 millones de los que padecen hambre y 11 millones padecen hambruna. Más de la mitad son de poblaciones indígenas.

<img src="https://i.ibb.co/NTrggwr/mexicohunger.png">

El crecimiento económico es clave para aliviar ese hambre, y ya en estas comunidades hay una plétora de cooperativas agronómicas ansiosas por vender sus productos.
Pero tienen varios desafíos, uno de los más importantes es lograr un comercio justo.

<img src="https://i.ibb.co/QFDnBJm/Slide4.png">

Y ese es uno de los más importantes, ya que descubrimos que la mayoría de estas cooperativas no tienen comercio electrónico y sus productos se venden en otras plataformas, en cuyo caso esto no llega a los productores, rompiendo el comercio justo que es devastador para esas comunidades.

<img src="https://i.ibb.co/0t97jc9/Slide5.png">

Ahora bien, ¿Qué busca un consumidor en una marca?

El consumidor moderno busca productos que sean:

* Socialmente responsables
* Orgánicos
* Saludables
* Sostenibles

Pero principalmente….

Que le generan confianza

La confianza es determinante en un entorno social, como podemos ver en este ejemplo de Nike relacionado con una causa muy social.

<img src="https://i.ibb.co/dJLMmTk/Slide9.png">

Entonces, nuestro objetivo es crear una plataforma que AYUDE a los productores aumentando la confianza del consumidor y mejorando el comercio justo.

# Traction:

Ya hemos dado algunos pasos al:

<img src="https://i.ibb.co/HDzgTxX/traction.png">

La cooperativa en se llama Obio que trabaja en el sureste de México y está respaldada por uno de los bancos más grandes de nuestro país y del mundo.

Aquí están los enlaces que muestran la prueba social de estas declaraciones:

https://www.talent-republic.tv/imperdible/blank-ganador-indiscutible-del-talent-hackathon-citibanamex-2021/#:~:text=Blank%2C%20ganador%20indiscutible%20del%20Talent%20Hackathon%20Citibanamex%202021


https://www.youtube.com/watch?v=QAhZB9Bro_0


Aquí hay un video adicional del camino y nuestra primera entrevista con los miembros en cuestión:

https://youtu.be/IQ4431EjPZI

Más sobre Obio:

https://www.obioorganico.com/obio-y-asociados/



# Validación

<img src="https://i.ibb.co/5h78njw/Slide19.png">


Dicho esto, buscamos que nuestros certificados garanticen que la compra beneficia a quien la produce.

Gracias.


# Solution:

## DEMO:

Video: Click en la imagen
[![DEMO](./Images/logo.png)](https://www.youtube.com/watch?v=fDPsVpeqT7I)


### Desde este punto es la documentación técnica (solo Eng)

## How it Works:

The entire platform was primarily based on IBM Cloud services.

<img src="./Images/diagram.png" width="80%">

1. The application stores the data of the products through a Cloudant database.[Details](#cloudant)
2. The database is read by the web page through an API, it executes a Python-based Cloud Function to perform the DB Query.[Details](#api-and-actions)
3. We embeded the chabot on the website through the IBM Web Chat integration as a scipt.[Details](#chatbot)
4. The web page is deployed through a CI / CD cycle thanks to an IBM toolchain.[Details](#toolchain-cicd)
5. The page verifies the provenance of the products by reading the data in the Solana blockchain.[Details](#solana-blockchain-integration)
6. The application can check out the cart through the Rapyd APIs.[Details](#rapyd-integration)

NOTE: If you want to replicate this project you need to have an active account in IBM Cloud.

[IBM Cloud Create Account](https://cloud.ibm.com/registration)

## Cloudant:

For ease and speed of implementation, it was decided that a non-relational database was ideal for this project and for storing the platform's products.

The database was provided by the cooperative [Obio](https://www.obioorganico.com/), the products displayed on the platform are real products from real Mexican producers.

<img src="./Images/db.png">

The documents stored in the DB of the products are as follows.

<img src="./Images/db-product.png">

## API and Actions:

In order to safely deploy the database on the website, an API was implemented.

<img src="./Images/api.png">

Each route of the API is linked to an action, which aims to obtain the entire database or obtain only a part with a query. Both were implemented with a Python 3.7 runtime because of its ease of use. We leave here an example of how we did the query in a part of the DB.

/getDB-label

    from ibmcloudant.cloudant_v1 import CloudantV1
    from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

    authenticator = IAMAuthenticator('XXXXXXAPI_KEYXXXXXX')

    service = CloudantV1(authenticator=authenticator)

    service.set_service_url('https://XXXXX-XXXXXXXx-XXXXXX-XXXXXX-bluemix.cloudantnosqldb.appdomain.cloud')

    def main(params):
    response = service.post_find(
        db='products',selector={
        "_id": {
            "$gt": "0"
        },
        "labels": {
            "$eq": params["__ow_headers"]["label"]
        }
    },
        fields=["code", "name","brand","classs","price","weight","description","stock","origin","details","images","labels"]
    ).get_result()

    return({"data":response["docs"]})

This API is executed every time we select a product category in the application.

<img src="./Images/selectp.png">

If we select, for example, honey, the action will only query the products that have honey as their label.

<img src="./Images/honey.png">

## Chatbot:

The chatbot was fully implemented using Watson assistant.

<img src="./Images/assistant.png">

This is displayed on the website and you can try it without any problem.

<img src="./Images/webassistant.png">

Its main functions are to provide information about coffee brands.

- To activate this Intent, please type one of the following or similar phrases in the chatbot.

  - Ask for coffee information
  - I want information about a coffee
  - Coffee information

<img src="./Images/info.png">

Another one of its functions is to carry out a test to obtain the ideal coffee according to your tastes.

- To activate this Intent, please type one of the following or similar phrases in the chatbot.

  - Coffee test
  - I want to know my favorite coffee
  - Test

<img src="./Images/test.png">

## Toolchain (CI/CD):

In order to display the page and that it could be used by anyone in the world it was deployed following the CI / CD methodology thanks to an IBM toolchain.

<img src="./Images/CC-deploy.png">

All version control was done through a repository hosted by IBM.

<img src="./Images/orion.png">

All frontend development was done with the ReactJS framework.

[WebPage](https://github.com/altaga/CoffeeChain-CFC/tree/main/Website)

## Solana Blockchain Integration:

The website can read the data directly from the solana blockchain to find the record of each product through its signature, which is encoded in a QR to be able to read it easily with the platform.

<img src="./Images/solana.png">

Reading and writing on the blockchain is done through the Solana API.

explorer-api.devnet.solana.com

This section of the page has 3 fundamental sections.

- The QR code scanner.

<img src="./Images/scan.png" width="50%">

- The information loaded from the blockchain, which includes the checkpoints, image, brand and other information related to the product:
  
<img src="./Images/infos.png" width="50%">

- The map that aims to be able to see the locations where the product has been by checking the distribution chain, in addition to it we will have a link to the blockchain explorer to be able to see the information loaded in it directly, it should be said that this information it is permanent and impossible to change.

<img src="./Images/blockinfo.png" width="50%">

Here we leave you the QR of one of the products so you can check it yourself.

<img src="./Images/product.jpeg">

## Rapyd Integration: 

The Rapyd checkout is one of the most important parts of the marketplace, since it gives you the ability to make payments with real money to buy the products, in this case the API that we implemented in the platform was to be able to perform the Checkout of the products.

In that case, the API is executed once we have finished selecting the products and press the Checkout button.

<img src="./Images/cart.png">

We can see that once we press the Checkout button it takes us directly to the Checkout page that Rapyd gives us.

<img src="./Images/check.png">

As part of the Rapyd implementation we can easily select and add all the payment methods that we think are convenient for our business.

<img src="./Images/method.png">

# References

1 W3C, 2010 what is provenance https://www.w3.org/2005/Incubator/prov/wiki/What_Is_Provenance 

2 Lofty, What Is Provenance and Why Is It Important,
https://www.lofty.com/pages/what-is-provenance-and-why-is-it-important#:~:text=Provenance%20is%20the%20history%20of%20the%20ownership%20and%20transmission%20of%20an%20object.&text=Experts%20are%20interested%20in%20the,that%20an%20item%20is%20authentic. 

3 Provenance Proof https://www.provenanceproof.com/provenance-proof-blockchain 

4 Business Wire, 2021 https://www.businesswire.com/news/home/20210401005352/en/Global-Craft-Beer-Market-Report-2021-Market-to-Reach-554.3-Billion-by-2027---U.S.-Market-is-Estimated-at-44.3-Billion-While-China-is-Forecast-to-Grow-at-24.2-CAGR---ResearchAndMarkets.com 

5 Business Wire 2021
https://www.businesswire.com/news/home/20200513005323/en/Global-Coffee-Market-2020-to-2024---Insights-Forecast-with-Potential-Impact-of-COVID-19---ResearchAndMarkets.com 


6 PRN News Sire Far More Than Walmart China -- How VeChain Leads Blockchain Adoption in the Food Industry Around the Globe
https://www.prnewswire.com/news-releases/far-more-than-walmart-china--how-vechain-leads-blockchain-adoption-in-the-food-industry-around-the-globe-301320495.html 

7 VeChain Foundation, 2021, Walmart China Takes on Food Safety with VeChainThor Blockchain Technology
https://medium.com/vechain-foundation/walmart-china-takes-on-food-safety-with-vechainthor-blockchain-technology-b1443e0e079c 
8 Comunidades Fuertes, Territorios Vivos, UCIRI nos llama a valorar y a consumir el café campesino y justo
https://www.ccmss.org.mx/uciri-nos-llama-a-valorar-y-a-consumir-el-cafe-campesino-y-justo/

