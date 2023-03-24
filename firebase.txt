// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getFirestore, collection, getDocs,
   addDoc, deleteDoc, doc, onSnapshot, query, where, orderBy, serverTimestamp, getDoc, updateDoc } from "firebase/firestore";
import { getStorage, ref , uploadBytes} from "firebase/storage";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
const firebaseConfig = {
  
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const db = getFirestore();
const storage = getStorage();
// collection ref
const colRef = collection(db, "seller");
const stroageRef = ref(storage, "images/squidi.jpg");
console.log("storage", stroageRef)

// queries
// const q = query(colRef, where("firstName", "==", "tom"));
const q = query(colRef, where("firstName", "==", "tom"), orderBy("firstname", "desc"));
// const q = query(colRef, orderBy("firstname", "desc"));
//get docs (get data once)
getDocs(colRef).then((snapShot) => {
  console.log(snapShot.docs[0].data())
  console.log(snapShot.docs[0].id)
}).catch(err => console.log(err.message))

//get realtime docs (every time database updata this gonna run)
onSnapshot(colRef, snapShot => {
  console.log(snapShot.docs[0].data());
  console.log(snapShot.docs[0].id);
})

// get data from query
getDocs(q).then((snapShot) => {
  console.log(snapShot.docs[0].data())
  console.log(snapShot.docs[0].id)
}).catch(err => console.log(err.message));


function addDocs() {
addDoc(colRef, {
  firstName: "tom",
  lastName: "james",
  sellerRating: 3
  // createdAt: serverTimestamp()
}).then(() => {
  // formref.reset()
})
}

function deletesellers() {
  const docRef = doc(db, "seller", "sellerId");
  deleteDoc(docRef).then(() => {
    console.log("deleted")
  })
}

// get a single document
const docref = doc(db, "seller", 'cjxsQIQwRGZ1VgfG2P5y');
console.log("docred", docref)
getDoc(docref).then((doc) => {
  console.log(doc.data())
})

//real time single doc

onSnapshot(docref, doc => {
  console.log("hi")
})

// update document
function updatedoc() {
  // refrense for doc
  const docRef = doc(db, "seller", "sellerid");
  updateDoc(docRef, {
    firstName: "james"
  }).then(() => {
    console.log("update complete")
  })
}

export function addImage(image) {
  const storageRef = ref(storage, "images/picture1.jpg");
  uploadBytes(storageRef, image).then(snapShort => {
    console.log(snapShort);
    console.log("upload completed")
  })

}
