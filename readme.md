# Many iteration options but few perfomant

Let's take this scenario: for a string S, firstly, print each vowel (english) of it on a new line and then 
print each consonant. Each character is listed in the order they appear in S).

We will loop string chars and if it's a vowel we display it otherwise we add it to c_list. Here's a list of loop timing results:
```
let str = "lovely tokyo 🗼 buildings, people 🧑 and 🥡...";

let c_list = [],
	vowels = ['a', 'e', 'i', 'o', 'u'],
	str_list = [...str], // or
	// Array.from(str) 
	// str.match(/./ug)
	// str.split('') - doesn't work with unicode chars
	num = str_list.length;

console.time();
for (let i = 0; i < num; i++) {
  console.log(str[i]);
}
console.timeEnd();
// ~ 7.61ms

console.time();
for (let i = 0; i < num; i++) {
  console.log(str.charAt(i));
}
console.timeEnd();
// ~ 8.04ms

console.time();
for (let char of str) {
  console.log(char)
}
console.timeEnd();
// ~ 8.69ms

console.time();
str.replace(/[\S\s]/g, (char) => {
  console.log(char)
});
console.timeEnd();
// ~ 9.3ms

console.time();
str_list.forEach((char) => {
  console.log(char)
});
console.timeEnd();
// ~ 8.3ms

console.time();
str_list.map((char) => {
  console.log(char)
});
console.timeEnd();
// ~ 8.32ms

let it = str[Symbol.iterator]();
let theChar = it.next();

console.time();
while(!theChar.done) {
  console.log(theChar.value);
  theChar = it.next();
}
console.timeEnd();
// ~ 8.24ms

let it = str[Symbol.iterator]();
console.time();
for (let v of it) {
  console.log(v)
}
console.timeEnd();
// ~ 12ms
```


The conclusion: keep it simple and fast with the classic "for" loop for strings with normal chars and "for of" for unicode ones
```
for (let c of str) {
  if (c != '') {
	  if (vowels.indexOf(c) > -1) {
	  	// or vowels.includes(c)
	  	console.log(c);
	  } else {
	  	c_list.push(c);
	  }
  }
}

let c_num = c_list.length;

for (i = 0; i < c_num; i++) {
	console.log(c_list[i]);
}
```
