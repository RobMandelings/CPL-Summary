
---
# Fixed set of members, static member name
class: {member1, member2}

objectA.member1
objectA.member2

---
# Fixed set of members, computed member name
class: {member1, member2}

a = “member2”;
objectA[a] → gives back member2

---
# Variable set of members, computed member name

objectA[“member1”] = {} → assign an object to a new member
objectA[“member1”] → gives back the new member of the object!