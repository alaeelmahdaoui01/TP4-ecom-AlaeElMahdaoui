<template>
  <header>
    <img src="https://img.freepik.com/vecteurs-premium/pieces-automobiles-engrenages-bougies-allumage-piston-automatique-cle-anglaise-conception-du-logo-pieces-automobiles-details-automobiles_681147-1173.jpg" alt="LOGO">
    <h1>Les Bonnes Pièces</h1>
  </header>
  <main>
    <section class="filtres">
      <h3 class="filtrestitre">Filtres</h3>

      <div class="filtre">
        <div><input v-model="filtreNom" type="text"></div>
        <div>
        Categorie:  
          <select v-model="categorie">
              <option value="Toutes" selected>Toutes</option>
              <option value="Moteur">Moteur</option>
              <option value="Filtration" >Filtration</option>
              <option value="Électricité">Électricité</option>
              <option value="Freinage">Freinage</option>
              <option value="Refroidissement">Refroidissement</option>
              <option value="Suspension">Suspension</option>
              <option value="Sécurité">Sécurité</option>
              <option value="Transmission">Transmission</option>
              <option value="Carburant">Carburant</option>
              <option value="Carrosserie">Carrosserie</option>
              <option value="Échappement">Échappement</option>
              <option value="Direction">Direction</option>
          </select> 
        </div>
        <div>
          Dispo:  
          <select v-model="disponible">
              <option value="Toutes" selected>Toutes</option>
              <option value="Disponible en stock">Disponible en stock</option>
              <option value="En rupture de stock" >En rupture de stock</option>
          </select> 
        </div>

        <div>
        Tri des pièces:  
          <select v-model="tri">
              <option value="croissant" selected>Croissant</option>
              <option value="decroissant" >Decroissant</option>
          </select> 
        </div>
        <div class="butt">
          <button @click="affichage='panier'">Panier</button>
          <button @click="affichage='decouvrir'">Découvrir</button>
        </div>
      </div>
      
    </section>

    <!-- Fiches produits -->
    <section class="fiches">
      <div v-for="piece in sortedpieces" :key="piece.id" >
        <div v-if="(((categorie !== 'Toutes' && categorie === piece.categorie) || categorie == 'Toutes') && ((disponible == 'En rupture de stock' && !(piece.disponible)) ||(disponible == 'Disponible en stock' && piece.disponible) || disponible == 'Toutes') && (piece.nom.toLowerCase().indexOf(filtreNom.toLowerCase())>=0 || filtreNom=='')) && ((affichage == 'decouvrir') ||(affichage == 'panier' && piece.panier==true))" class="piece">
          <div class="piecenom">{{ piece.nom }}</div>
          <div class="pieceprix"> Prix : {{ piece.prix }} dhs </div>
          <div><img :src="piece.image"></div>  
          <div><button @click="ajouterauPanier(piece)">Ajouter au panier</button></div>
        </div>
      </div>
    </section>

  </main>
</template>

<script>


export default {
name: 'App',

data() { 
  return {
    affichage : "decouvrir",
    panier:[],
    disponible:"Toutes",
    filtreNom: '',
    categorie: "Toutes",
    tri : "croissant",
    piecesauto: [{
      "id": 1,
      "nom": "Batterie 12V",
      "prix": 120,
      "categorie": "Électricité",
      "disponible" : true ,
      "image":"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQy9uVcxs8t393-l1bIYMbSeLeShmnpMalk5w&s" ,
      "panier":false,
  },
  {
      "id": 2,
      "nom": "Filtre à huile",
      "prix": 15,
      "categorie": "Filtration",
      "disponible" : true ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcREVmzzy35931qLEECHVzyXs3BG1YvmY25a-A&s",
      "panier":false,
  },
  {
      "id": 3,
      "nom": "Bougies d'allumage (x4)",
      "prix": 35,
      "categorie": "Moteur",
      "disponible" : true ,
      "image" : "https://www.revue-technique-auto.fr/img/fre/operation/sommaire/7.png" ,
      "panier":false,
    },
  {
      "id": 4,
      "nom": "Disques de frein (x2)",
      "prix": 80,
      "categorie": "Freinage",
      "disponible" : false ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ-aeCpJBvqLVUSVUCaVZGGLjq0kCLHe3oP7w&s",
      "panier":false,
  },
  {
      "id": 5,
      "nom": "Courroie de distribution",
      "prix": 90,
      "categorie": "Moteur",
      "disponible" : false ,
      "image" : "https://www.monpetitmecano.fr/img/cms/courroie-de-distribution.jpg" ,
      "panier":false,
  },
  {
      "id": 6,
      "nom": "Pompe à eau",
      "prix": 70,
      "categorie": "Refroidissement",
      "disponible" : false ,
      "image" : "https://www.bricop.ma/images/thumbnails/1086/1000/detailed/36/bd91c3491cc58fcc70d17fd2215911fe.jpg" ,
      "panier":false,
    },
  {
      "id": 7,
      "nom": "Amortisseurs arrière (x2)",
      "prix": 150,
      "categorie": "Suspension",
      "disponible" : false ,
      "image" : "https://www.accessoires-bmw.fr/69097-large_default/amortisseurs-arriere-x2-pour-bmw-serie-3-gran-turismo-f34.jpg" ,
      "panier":false,
    },
  {
      "id": 8,
      "nom": "Filtre à air",
      "prix": 20,
      "categorie": "Filtration",
      "disponible" : false ,
      "image" : "https://ma.jumia.is/unsafe/fit-in/500x500/filters:fill(white)/product/56/929695/2.jpg?4845" ,
      "panier":false,
  },
  {
      "id": 9,
      "nom": "Capteur ABS",
      "prix": 50,
      "categorie": "Sécurité",
      "disponible" : false ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSBismHoxP6nuA5eQJczYfwruNhOtRjbUjIkw&s" ,
      "panier":false,
    },
  {
      "id": 10,
      "nom": "Radiateur de refroidissement",
      "prix": 130,
      "categorie": "Refroidissement",
      "disponible" : false ,
      "image" : "https://assets-valeo.keepeek.com/medias/domain8143/media103031/909376-7vjmv3l3a9-preview1.png" ,
      "panier":false,
    },
  {
      "id": 11,
      "nom": "Alternateur",
      "prix": 200,
      "categorie": "Électricité",
      "disponible" : false ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSFv6nGWDS5OWaPlLmHkHaBqvVn_JdwSSwJ3w&s" ,
      "panier":false,
    },
  {
      "id": 12,
      "nom": "Démarreur",
      "prix": 180,
      "categorie": "Électricité",
      "disponible" : true ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQVtOCJyabMREPjQPVagsIx76DU46YFnPO9QQ&s" ,
      "panier":false,
    },
  {
      "id": 13,
      "nom": "Kit d'embrayage",
      "prix": 220,
      "categorie": "Transmission",
      "disponible" : true ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
    },
  {
      "id": 14,
      "nom": "Injecteur de carburant",
      "prix": 110,
      "categorie": "Moteur",
      "disponible" : true ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ747Tdd4rxFsSICjvSrTcRdRhwgSrRpZCzwg&s" ,
      "panier":false,
    },
  {
      "id": 15,
      "nom": "Pompe à carburant",
      "prix": 90,
      "categorie": "Carburant",
      "disponible" : true ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
  
    },
  {
      "id": 16,
      "nom": "Capteur de pression des pneus (TPMS)",
      "prix": 45,
      "categorie": "Sécurité",
      "disponible" : true ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS1gOeKODhTh2mh7piB9E3KFz62zs7RnOGs3A&s" ,
      "panier":false,
    },
  {
      "id": 17,
      "nom": "Rétroviseur extérieur",
      "prix": 60,
      "categorie": "Carrosserie",
      "disponible" : true ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSroqUvwlX557PeYcBFnLjZ2Eko0gGCCVIT2g&s" ,
      "panier":false,
    },
  {
      "id": 18,
      "nom": "Échappement complet",
      "prix": 250,
      "categorie": "Échappement",
      "disponible" : false ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
    },
  {
      "id": 19,
      "nom": "Vanne EGR",
      "prix": 140,
      "categorie": "Moteur",
      "disponible" : false ,
      "image" : "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQY1k-bLxcHqYFBLELhMJNmsPmra42IuRZ07A&s" ,
      "panier":false,
    },
  {
      "id": 20,
      "nom": "Turbo",
      "prix": 400,
      "categorie": "Moteur",
      "disponible" : false ,
      "image" : "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBxAQEhUQEBITFRIVFhIQFRUQEhIVEBUWFhYWFxUVFRUaHSggGBolHRUWITEhJSkrLi4uFx8zODMtNygtLisBCgoKDQ0NFQ0QFSsdFRktKy0rLSstKy0tKys3LSstKzc3Kys3NystKy03NystKy0rKystLSsrKystKysrKysrK//AABEIAMgA/AMBIgACEQEDEQH/xAAcAAEAAgMBAQEAAAAAAAAAAAAABQYDBAcBAgj/xABFEAABAwIEAwUEBQgIBwAAAAABAAIDBBEFEiExBkFREyJhcZEyUoHBQnKhsfAHFBUjgqLR4RZTYnOSssLxJCUzNEOz0//EABYBAQEBAAAAAAAAAAAAAAAAAAABAv/EABgRAQEBAQEAAAAAAAAAAAAAAAARARIC/9oADAMBAAIRAxEAPwDuKIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICLFLO1u516AEn0C9bO08/XRBkRYxOzXvN03sRcea1565rf5oNxFW6vH7aNuT4LROKVL/ZFkSrkiqDZ63k8rIzFK1m4a7zb/BCrWir8HEltJYnN8Wd4ei2f6R0/vH0KKl0UP/SWm94/4VnpMbp5Tla/vHYOFr+XJBIoiICIiAiIgIiICIiAiIgIiICIsc87WDM42H42QZEUBiXETI2lxcGNHN258lQ8Z4/leS2nBA995J9G7BB1tVvi7iN1GY2MjzOkDyCXWALbabH3guTPxSrmd+snkJOzWE/5Wrdczs4HyvLz3XZe0JJuRYWvtc/cg6bHWd0C4J5nYk81G1tVPI8QQ6OIu551DG9fE9Aq3wz2tXFG4E3IGY9DzKusUTYGWG/MncnxKrLFBDFSMyt1J1c4m73u6uPNaL3STHwWQRuldc7KVghawINajwpo1ct3tYY9Li+ug1OgufsC06+oc3v6FgtmBvtcXPjpfQrULcrtToHmRpGW2rcrm8rdduZREk7FGGwaLklg32ztLmk+YCwnFI9Lgi+S+YOBBk9gEEXBPQgWUWy4DGl98gjsDcgFgtca6XFrjwXnZgWy2sAwW1IGS5YRruLne6Kloahsri1jRlbbMSOuwAWSXDInclF4BWMZLJE8hufI5hcbAkDK5t+ugPxUxJUi/dNx1GyI0BhMeott+AsjcEjKzU813u8h81vRyBCtuhcSwZjcjuk9baLYWnhz75/rfIFbijWCIiKIiICIiAiIgIiICIsFdVshjdK82a0XPyA8SdEHzX1rIGF8hsBy5nwC5/i/EheS8n6rRsFpcQ4zJO+zvMjkOjfht53UPUwGzQd36+Tf5oNOrnkqSXyO7g2HIKInqLuyR26knZo6lbOOVuW0Uep0aANyToAFEzuDB2bTc3u8j6TvDwGw9eaCQw58kk0cMIGpGdzubRq8vt9G1+7foFJcZYg+eZtLCC45msAG7pDZrW/C4H+y+cFApKaSqPtuGVvgL2A+LtT4NClPyUYR2s8ldILiK7GE85XjvO8w02/bPRB0Th3BmUVOyAalrRnd7zranyXxWOL3WUlUOytUfA2/eKrLNTtDQvXSczsvhz1UeMOJm07DrrsAPaceQA6oJLGuIY4gRcKlVnGhJ7gJ+qCR9irVTJLKc87i2+rYmau/ady+C2aTD3utZtr6DVznevNRYkW8XP8ApAjzBH3qWoeJg7cqPrcMggbkraxsLiL9k0GapynmY26tB6krVoaXB3GwrqiPkC+mOW/LbYX/ANxugu1LXsdropWKrCpbMEq4gHwj85hPsy0t5GuHi1t3NP4upmgp6sjWnnH1opB9hCosUdQBqvv88UU6Kcbwyjzjf/BbmHYRUTEZmOjZzdIMrvINOvqiRY+H7lhefpO0+H4+xSixwQtY0MaLACwWRRoREQEREBERAREQEREBU7jqv7zIBs0du8dbaRtPx+8K4rlPEVZ2k0zr+1IWD6sen290oNPDaQyyAHmbn5rDilQM0knIdxvkNApnCB2cUs3usNvM6Kp113iOEGxe7U9L7lBXWzd6Sc/QAYz+8kzWPwa1588qw4dEZHab3a0ebjYH4bqaxXAXZMkRaGMa6oke8kN79mNBsCSe5+8sXClEQDUPFoosxzHYvItp1s0+rx0QZ+LqgNEdOzZjQbePssHna/8AiXV+CMNFPSRQ87Z3+Lnd532lchwuI1dT2rh3M2bXbo1vwFvQLumE+yD4fy+SrPpjxN+oaFitYWSZ2aRfMz0EZjleImEk20J8Fy6drpWHEJh3XudFSMdu63tyW6DmfFo5qd48nfO+KijPfqJGxacm3GY/aPhdaePPbLVdjGLQUrRSRNGwyaPPmXX155QorTwnDsxzO1J1JK2+MOIxhbBT02X8+e0Oe82P5rG4XaAP65wN9fZBHVTVC6OmikqpReOnjdOR7xGjGftOLR8VyCncaiSWurCXNzmR/WaVxJbC3w69GjloissGYMDp3kyTEvbmN3hp/wDI9x1OY6/bzXy1xabHcaLUqKl0z3SPN3ONzbYdAByA2W3J3msfzILD5t+ZBBQWnhDiWbD5BLGSWE/rIr9145kdHdD8l+iMMr46iJk8Rux7Q5p8D16HwX5YjPdb8V2P8ieKExy0jj7BE0f1XXDwPAOF/wBtB05ERAREQEREBERAREQEREBERB8SvytLjsAT6C64oZS7JfcguPmXG5+wLsONvy007ukUp9GErizHd5v1Wf5Qfmgslc7JREc3uAXP8criyQEfRsrvxBJaCFvU3XMsclu93mgn8LqhUVDGS3dE6BrnMuW5rEEAkG4FyNui6pRYZT1FOyOaFnZi5ZG3M2JrbnL3QdTbUk31JXIeDq2z8lh34QL8wYpDYeRB+wLsmFTjs2/VH3ImozGsHpaaPPHGWm9mhrrNFtemysWCS5oGO6safUX+ag+LXZo2/tfcpHAJP+GZ9Vv3BVGSN3eJWGtfZpK+oTqVq4qe6UVQsKmD8Y7R2raaGWW3K4Yf/oPRR+BsLu87VxOYnqTqSveH3Xq6/qaeoaPRg+SzYDsFFbfHlR2OGtYACaioZG7MARkiY6TY6Hvlm65zVUE07GNYABGHZWNaGtJcbudYaZjoPIDZX/8AKVA99NRZG3Akq81tTe0GXTnzVXocffh5Y/IM5GeO4DmmxIuDtoRYjcdEFbp8LqTtTzn6sEpHqGqTbh0zYbPikYe0uO0Y5hILbE2cB7oWKs4gq55HyunkBe4vLWPc1gvya0GwCy/nMhgBe97i6V1i9znEtaxu1ztdxQHCwA6K9fklnLcQYB9OKVh/df8A6FQ4hdX78ktPmxBjvcjlf9zf9YQd0REQEREBERAREQEREBERAREQRvEv/aVP9xP/AOty4zH7Y8mD0aAu6TxB7XMdq1wLSPAixXFcQpOwqZIQSQx2QE2uQLAE+NkGfiSXuxDoCub4gbuPmVeuIZb5fAKg1R7xQbuCxOjiNW8uZFG8saW2zSucP+m2+wGW5d5212kMI44qWjJdptci+m5vusvB9QJ4Z6KoBNOG9rn/AKok7A8iT3gOoKpUrTG7XdpsfEIOl0vGr5j2UjP3vhcaLomASWha3+yPusuBNqCC2Ubg3Pw3/HiuwcL4iHRtsdPkdfmibizxHVa2LDulfcT9Ur23BVHK8DlEWKPa7aQujPk9gt9tluYawxPdGd2OLPQ2UXxdA6GpbM3TbXoWm4Kmq6YShlZHtIA2QD6MgFjfz+Xior6/KPmfhJyi/ZVML3+DHtc2/wDjyD4qi4EGVMf5nIQ3Oc9O87RzWsGn+w/Rp8cpXTaHsqiOSmmP6qeN0Ljocub2Hjxa4Nd8Fx6sopqWWSnmblkjcWOHK42LTzaQQQeYIKDHNC9jjG5rg9pLC23eDgbFthubqTxBoYWQDXsm5XEajtHEukseYubfBb1VVdoGVb2n84a0McTaznDSOZw97KPUX6KLhYSbnfx3QbkTNB4rr35GsMIE1URocsLT5d5/w1Z6Fc1osPkqJIqeFt3us0dNdS5x5ADUnoF+iOH8LZSU8dOzZjbE83OOrnHxJJPxQSCIiAiIgIiICIiAiIgIiICIiAuQcVM/5hNbq0/utXX1x2vmE9dM8agyOA8QNB9yCIx1uyqMGHmeoZADlzuy5iL5RqSbc9AdFdOKm5XBvhdQeEsy1TJP7L7eeUoJCoZGxgggblhabi+r3u5ySHm4+gGgVR4jorESDY6H5FW6RadfTCRhaeYQUmifuw/D5K7cE4nZvZk6t0+HL5j4Kh1DCx3i02KkcOrTG9srdtnAfj4oO50lTmAK3y64VMwPEw5oIOh1CstPUXRFd4vwvtGkgXI1VOwevfTFzCC+J2j2HceNuo6rq0rQ8WKqOO8KFxzxaO8N0K1KRwPegcHs92/6xvgQtvFKCDEGt7U9lVRtyRzuabFo2jlHQcnbi/mDA/omuabC9+uRt/Wyl8H4PqpTeaSTLzBe63peyFQdVwfXxNLTA6QF2bPB+tY6w0Iy3NtTuAtrBuBK6ZwHYmMe9Ndlv2faPwC6fh+Gw0jLDl6KxYDTWb2zh3ngZR0Zy9d/RDNqP4R4RhoG5vbmIAdI4AG3utH0W35ep2tZURFEREBERAREQEREBERAREQERfD0HPOJeOJI55qXJYMOQOa7UgtBva3j1VWwyUMdnZZ3OxNirrxTwrDUyOkOZkht3mHewAFwdCqbVcL1cBvHllA905H+h0+1BA8ZcQB8wGTKbBpB5Af7rVw+qjADi6zw4ADcODu6SCNrXUhjPavsJYJCRp3oS794A39VX6uN1rNhkH1YHj7moLFKV8By+A8uaHEEEgEgizgbbWOxUNDHXOlDnANaT7GYbchYDU/FBqcTUdj2g2Oh8+ShaB1nFvI6ev8ANXSsYJGFh5hV52F5QHNuTz66fgoJnAqh0TL65Ra/he6uWHYqCBqqxhXZviLWnvXuQdDtpp4arDJG+I3YbeHJB0WKsB5rcirRzXPqbEJmjvBt+jXX+8BbjMYf7p9R/FEi/sro/BeTYy1o0PoqJ+lHnoPNw+4XWjVQ1NSQGyljb/RABd5nf0Vqcr0Kp0xufZ5Dqr5hji2JjXbgC/8ABUbhqHJlMjszxaxIs3z81coJFFzEmHL2612FZgUV9ovAvUBERAREQEREBERAREQF4QvUQYJYQ7Qi6hKqmsSFYlpVMQLkFYqqe/JRdTR+CuUtKOi1paJvRBzDGcMdfOwX6gb6cx1/koWWSV7rAWtyDSDfqST/AAXXpMKY76I9FqnAYgdGC/kg5czCnnf7P4ryXBHnT7l1M4M3osf6Jb0Qco/ou8nMHOaerTYrcdw7K9uV73OHjYX87BdQbhTei+v0aOiDmUfDb7BuZ1hpYaLZg4UHMeq6O3DR0WZtAOiCjU3DrRyUrSYSG8laW0Q6LK2jQRlNSKdpobALGynspGIXCDxjVkAXoavbIC9REBERAREQEREBERAREQEREBYcqzL5sgxPYsTo1tOC+C1BhESx9ityy8sg1HRL4EC23BMqDV7FeiBbQYvcqDWEK+hCtgNQhBh7MJZZLL6DUGMNWRgsvcq9AQfSIiAiIgIiICIiAiIgIiICIiAiIgIiIC+bIiD2y8KIg+bL0BeIg+wEsvUQF8IiD2y9siICIiD1ERAREQEREBERAREQf//Z" ,
      "panier":false,
    },
  {
      "id": 21,
      "nom": "Joint de culasse",
      "prix": 75,
      "categorie": "Moteur",
      "disponible" : true ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
    },
  {
      "id": 22,
      "nom": "Boîtier de direction",
      "prix": 300,
      "categorie": "Direction",
      "disponible" : false ,
      "image" : "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBxIREhIQEhIWFRUVFhcVFRAXFRcaGBkYFRcYFxUVGBcYHSggGBolHBgaITIhJiktLi4uFx8zOjMtNygtLisBCgoKBQUFDgUFDisZExkrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrK//AABEIALcBEwMBIgACEQEDEQH/xAAcAAEAAgIDAQAAAAAAAAAAAAAABgcEBQEDCAL/xABCEAACAgEDAgMFBAcFBgcAAAABAgADEQQSIQUxBhNBByJRYXEygZGhFCNCUmKSwQhDU7HRFSRygqLwNXODstLh8f/EABQBAQAAAAAAAAAAAAAAAAAAAAD/xAAUEQEAAAAAAAAAAAAAAAAAAAAA/9oADAMBAAIRAxEAPwC8YiICIiAiIgIiICIiAiIgIiICIiAjMw+o6+ulS9jYAwT69zgfn/kZgN4joFiVbiWbGMYPBYKCeeBkwN3mJ0WahV+0wH1M1+q67QmQW/yA/MgQNvEgHWfaTp6MjcM9gqqzN9TggAfWQzWe1F7WwiuB+82Dj6ICB+MC6hqk3bN67h3XIz94nfmefura9xStj3MyWbiEA24K9zgYA+nOZansw1V1vT6LbiSW3FNxywr3EVhj6nAz94gS6IiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiJxugcxPndNb1DxBpaDttvRW/czlv5VyfygbPMZkU1HjId69Nay/4tmKk+vvnOPukY6j4+fJH6VQhPAqpU2vn0AY8EwLRZwOSQJpdb4q0dbFDerOO6IC5z8CEBwfriVbfq3uOTTqLT3L6iwqgA5z5a8AD6SNdb8VvYvk07aq/2nrXZu57Ke4T8zAlvjrxMl9reWWTaiJ5ZIy/vMW3BSQuARwTk7vlOnxPq7a9NpNWvuhqdgbKg7twIwM5JGM5xiaPo3gvWXVC1F2FhlFZGBPOMsxAC5H1OMfGSt/B9Vex72G1AQEcmw/xbV4UZMCH6TxvrGYK1itnuxUbv5hzM3xbrGSui1G2eYGLtnJyDjue0+PEF2hDErQoH7+AD8sdsfdI71TSmtVu2jA7I7bmCsAVKoTnHOc/AiBqNRazBmXAX1tdgMk+gDHLevPMzOj+E+oalRdRWTUSR+ktYi18HBILYJH0E1Gtsa1RazZIJUDAGAOcAAfEiSzwv03V2pRURYNMXJLjDp7x5IQHls8c9oEn0HgjzK6Ut1b2YHOxl8oE/bCtgErx3PMubT3011KwdFrCjDbgFAHz7SpOo+HhpKjZW9SinAZrbN7tuxkmvO1TjkAfSRbpnWD5+no095tqqdzXXsxUDZ75XA7rv3Yz6OPhA9H12qwDKQQexByDPuRnwZpWrW5ShRTYWCnOASBuCk91zJNAREQEREBERAREQEREBERAREQEREBERATWeIuqDSaa/VMMiqtrNucbtoJCjPYntNnKr9uHWH8unp6If1581rc8baTk17fUk4PfsvzgV34h8Ya3qFjf7w1KMm5KKrGRACpOH2kbmJB7/ACmH4f8AElmmVCLR5YOXUoosOQcr5mNzYPrMFqBTW9gGSfqcN2xzNItgJJcFs/sqQo+HoDA32s8RvfabLFNi5G2o22cAfEZIOfpMbqPVrS2xVWhB+xVlSex95h7zffx8vWa9XX3vdCZ/ZGTjj0Lc/wDfaSLw+1br5dSjzH4JYAsTjgDPaBm+C77gzJTpzZuQoeyjkHksZvOieF6dKw1OvsVyrBl01X6wAk8NZj4fP4SIdZ6jqCCHscYxlRhRx8lxOzwVqB+kBW4DZypLBHx6Mc5yM8fTEC0uteOTW5CVotbBRU9j7XdyQD+r2k7AM89+JFeo2G2yyjVam24Krve9HlhFQglK13Hce2MDmSHTayiqwPe+xK1IOwAYD8ABgN45X0IzxOrQUaU22dT0FFllnmrUtFpKJYbBgsDYCw5IIbvwcd4Ea6J4av6pW+opNCV1b0qRa+fd7B2Yk5I9STN3T4c/Qq084o7lWO7GSqjG5cn9nnI/5vjNqvR9QdlGnuTp2UL6kVAOC27naTjnOeT6SP29MGis82/WfpDNVZmtQWOzGATj4/68wMGmnp+mc3GhLKznC3HCAjcRs2jLcjkeoM+eqePKb6jSGWhATsNNJRR7vYrn97nPEhPizqK6i9rK1ZUwAqkdsDBOOw/+509KWl02lrTdvArprrVg2cYLMx4547doFldY/wBnqijWUt5nkqEbyyEUuObW+DEjPJzN17MtRpjqTdfYo1DA1U6cVbFSpGYq5/iYsxz85G9B0mqjOu6lcbLSdy1sd+GHY8/bbj6CR7rXWLeoamtE/Vh2FSKv2yGPJZhyT3+Q9PjA9RoP/wBn3MbQUeXWlf7qqufoMTJgIiICIiAiIgIiICIiAiIgIiICIiAiIgJVvt/rI0emtA5TVKCw7gPVaPwJx+MtKVz7bem6i/SV+Uu+tLN96DG7YFOHH/CeePjAqforJbW9b8Bhyf8AT5zU9V6fVTn9d5li4OAuF+OCTjPB+ExtFqMAkH4Y/pMbqFubGPxC8/VBA2uo6ZZYUvdRWtijHoMAbd2Phx3m/wCkeERprq7zqk/U3V7kKsA+dp9w/tAhu/yM0XVettq6NNQqCsULs35xuU+hz/Fz+M7eqW3L+jhr94r25GcbSp+Z5GIFgePvC6tTqNSgwUQuV9OCJXSa1EZSordTWApQnIL/AGkCt+2TwT2ljanx1XqdNbpqabr7LVaslKyUG4Y3buxxIb0nwwiXLTfZ5NhHHmgAY75Rz7h9OQeIG782rS2V1qpudmTdUCpb3CGPLkKORjn96Ov+L9YLg/kvpthBrpZVILqDhzkgkc/DEyNJ1/R6Sn/cqkfU+eaAW5dscs4Pcgr2PbJmo6oiv5tmu/SFsLNtsVQ1ezAO1/8ADIOe8DoVHRHu1OpIawtaw3kFucnAJ7ZOOBiSnwNoloqt6hqBt8wFlRu6U+mc/tPjP0ImkqarqD1WmmumnT8CxmwbBnI3byAB64ktXSaPWVNRZrq8kggLbWCCOwOTg/SBVfU+sb7XcIo3MzKgQe6D9kZmYviPUbNvlUgcYtZArLg9w39OZv8ApXs787XeXVqPM0212fVKvKmvaprAYbSxLryOMZ+Et7pHgXQafawoV3H97b77f9XAgUBR4c6l1BwyU22bsYfaUqA7Z3vgAfTOZbvs59madPYajUMtuo5C7c7KwePd3AFmP7xA+QljgRtgAs5iICIiAiIgIiICIiAiIgIiICIiAiIgIiICdV1YYFSMgjBHxz3H4TtnGIHlTxf0A6DWX6YHKKdyY9EfOwH4Y7fdNMQT2AJx6/CeiPaH7Pa+ohrkY16nZtVs4RwDna4wT8sjtKD1XTbKXeu1TXZWQHrbggn/ADB9DAyPDvQVsdTbkpnmpWwxA/i9JOeoP0fQuuKd1uA2xl8xl+HNnAPzkR0Wo2JWUGbN4BHO8AE7sAfskEfeJgdZ6gltzXOxJOMKnoB2GTx9cQJR1T2gOMrp6gP4nJIx6e6MdvrNPp+p67Wlq7dSUoX37mACIq9xkKOSfQEzA6VoG1GWVTsX7SV7TaR6EbuMfcZIV8PXX1pTlKE8vzv0UE79i5xZbn3mJwTk/LGIET6ZSvmgqdgBOLDnbkcrkjGCf6zYdf68145G8gBRa5LEbfslc9vgM57zo6gTUo0yk4bFtvbORyig4BxyM5+AmPo6sn387QMsB3OT9nntAk3h7q9nlANWoXso2bmbPBO0+nzM++qaXQLWHXSDeeXDMTwecDadqn6AgSVeA+gLco1TrhSNqV57qD3OBzn6+k3HWvDenvUooCjOGsHJyO4A9fhnMDnwR067RVrqNMxNFqBm0DsTtPfdVZ6HHpgA/LvLOqsDKGHqAfxlZeG/FdV156d9lq1/VuPs2CvAIAJJDAc459ZYXS290j0HaBnxEQEREBERAREQEREBERAREQEREBERAREQEREBERAYkf6z4R0WrY2ajTo7kAFyOSFOQMj4SQRA1Gk6BpqEdaaK68qR7qgenxnkvV07bbE/dsdf5WYf0nsppTXtL9lW8vrNAvvE7rNN+8Sfeas578klfX0gU9TeV5BKkdnUkMPoRLoW6qzp36ZtA1F2kG5wMnKoEbn6LKWTS2HOFJKn31x7y4OCCvfiWh4U6jUvTtONRYEQ7qFz3JJYbcfHmBWnU7zZda/bJ/IR0/cWC590sgY98BmClh8cAz661om02pel+6nB+BBHBHxBGD987UOwM38Of5WVv6QPQdoXS6VtnAqrwvHYD3QePxlf63xCbCmj02oWt7f78qSAAMqvyJJ7+mJYnu31EHmu1Ofo4/zlcaDw9/s62/U6kh0Xiu1RkYPxXurekDH8M+Gr6Op0uzbtlhLW5HI2HcR8QScS8OkPktjtxKW0njJrtQiUITuxWi/tHPb6ZPH0l19E0j1VKr8v3bHbJ9PugbKIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAnyy5n1ECDeNvZvp+oHz1/Uagf3yADd/wCYBgt9e8rPrXQep6EKmpq8+pTlNRUpfB+LqOfvxx8Z6Fny65/0geQvE2r821G4+wBkHIIznv8Af+c6+nndYqHncVX8WHP5S+Pal4F0DaXUa0VeVdUj2+ZXhd7KCQHXGGyfXv8AOUP0Mfr0b9zdYR/wKxH54gWT4e8WtXuqfLVb2K+pUFjxj1Hr8px4w6/VfpmqrclicnII4HOTn5fnIrpHCr8TjGZndG6Y2svr0yd7G5PwQHLsfuz9+IE+9jXgxKkXqFoLWMpFQPZQe7AfEjjP1+MtkTH0mlWpFrQYVFCqB8B2mQICInGIHMTjE5gIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiIEI9s+p8vpGp/j8uv8AnsUH8p576FVze3wr2/zuuPyUy6/7QWp26Cmv/E1KcfJFdj+YEp3odeKbDjl3xn5IvH5sYGWjD+mP6yyfYp07dbqNSRwgFSn5ty2Pux+MrVD8Zfnsx6Z5HT6MjDW5ub/1Dlf+nbAlYnMRAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBBicGBTH9orU/+H1fO+w/cK1X/wBxkB0KbaKV/hLn/mbIkn/tBajdrtNUP2NPn77LD/8AESOW2hX2eiKq/gsDu0GgN91OnA5sdU+gJ94/hmemKKwqqoGAoAA+Q4lK+ybQi7XG7GVpQtn4M/A/LMu0QOYiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgJwZzMHrXUq9LRZqLThK1LMfp6QKA9rtnmdade+xKE/LcR+cilupy7t/ET/3+U3L6l+o6+3WKjbmY2MgGQlajAJPyH5zZjwEx01epr1AYuAVQrjOfSBZHsV6Z5eia9hzfYSD/AAp7q/1lhia3w3ohRpdPSBjZUgx89o3fnmbMQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQErj28awp03ygcedaiH6DLEfgJYxmn8WeHqeoad9LcDtbkOMbkYdmUnsRArH2T6AU6HUap15dnz80qXAXHwJ3H7x8J2+BP0++s22eUmnKv5FIUbqyfskH8c5zJF4O8K6nQq+lsfzaUYtTaONyucsliejA57ZBz6Ykir6fhkVECpyGAGAPhwIGT4WrZdLUrkswBG4nJIycEn14m1nxUgAAHAHGJ9wEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERA//Z" ,
      "panier":false,
    },
  {
      "id": 23,
      "nom": "Silent bloc de suspension",
      "prix": 35,
      "categorie": "Suspension",
      "disponible" : false ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
    },
  {
      "id": 24,
      "nom": "Cardan de transmission",
      "prix": 160,
      "categorie": "Transmission",
      "disponible" : true ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
  },
  {
      "id": 25,
      "nom": "Capteur de position vilebrequin",
      "prix": 50,
      "categorie": "Moteur",
      "disponible" : true ,
      "image" : "data:image/jpeg;base64,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" ,
      "panier":false,
    }
]

  };
},
computed : {
  sortedpieces(){
    let sortedPieces = [...this.piecesauto];  // copie du tableau original ou let sortedPieces = this.piecesauto.slice();
    if (this.tri === 'croissant') {
      return sortedPieces.sort((a, b) => a.prix - b.prix);
    } else {
      return sortedPieces.sort((a, b) => b.prix - a.prix);
    }
  }
},
methods:{
  ajouterauPanier(piece){
    this.panier.push(piece);
    piece.panier = true;
  }
}
}

</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

#app {
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-align: center;
color: #2c3e50;
margin-top: 60px;
}

.filtres input{
margin-top: -20px;
padding : 12px;
font-size: 20px;
border-radius: 10px;
font-family: 'Montserrat', sans-serif,
}

.filtre{
display: flex;
flex-direction: column;
gap: 50px;
font-size: 20px; 
align-items: baseline;
}
.filtre div {
width : 300px;
}


.filtre select{
font-size: 16px;    
padding: 7px; 
font-family: 'Montserrat', sans-serif,
}

body {
max-width: 1200px;
margin: auto;
padding: 16px;
font-family: 'Montserrat', sans-serif;
}

header {
display: flex;
flex-direction: row;
align-items: center;
margin-bottom: 16px;
padding: 8px;
background-color: #6f2107;
text-align: center;
color: white;
}

.filtrestitre{
font-size: 45px;
}

header img {
height: 150px;
margin-left: 1px;
}

header h1 {
flex-grow: 1;
}

main {
display: flex;
flex-direction: row;
}

.piecenom{
font-size: 20px;
font-weight: bold;
color:#062e55;
}

.pieceprix{
font-size: 15px;
margin-top: 10px;
}

.piece{
margin-top: 20px;
padding : 20px;
border-bottom: 2px solid grey;
border-radius: 9px;
}

.piece button , .filtre button{
padding : 10px;
background-color: #6f2107;
color:white;
font-family:'Montserrat', sans-serif;
}

.butt{
display:flex;
flex-direction: row;
gap: 10px;
}

.filtres {
border-radius: 4px;
box-shadow: 0px 0px 4px gray;
margin: 8px;
padding: 40px;
min-width: 300px;
min-height: 400px;
display: flex;
flex-direction: column;
gap : 60px;
}

.filtres h3 {
text-align: center;
}

.fiches {
flex: 1;
margin: 8px;
}
.fiches img {
max-width: 200px;
}
.fiches p {
margin-top: 4px;
margin-bottom: 4px;
}

</style>
