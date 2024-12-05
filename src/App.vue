<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref, onMounted } from 'vue'

const cellsNames = {
  1: "a",
  2: "b",
  3: "c",
  4: "d",
  5: "e",
  6: "f",
  7: "g",
  8: "h",
  9: "i",
  10: "j",
};

let ships = [
    {
        length: 2,
        coords: []
    },
    {
        length: 2,
        coords: []
    },
    {
        length: 2,
        coords: []
    },
    {
        length: 2,
        coords: []
    },
    {
        length: 3,
        coords: []
    },
    {
        length: 3,
        coords: []
    },
    {
        length: 3,
        coords: []
    },
    {
        length: 4,
        coords: []
    },
    {
        length: 4,
        coords: []
    },
    {
        length: 5,
        coords: []
    },
]
// let field = ref([
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
//     [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
// ])

// function setField(row, col) {
//     field.value[row][col] = 1
//     console.log(field.value)
// }

// Math.floor(cell / 10), String(cell)[String(cell).length-1]-1

onMounted(() => {
    // placeShips()
})

let field = ref([
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 1, 1,
    0, 1, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 1, 1, 1, 0, 0, 1, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 1, 0,
    0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 0, 1, 1, 0, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 1, 0, 1, 0, 0,
    0, 1, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 1, 0,
])

let probs = ref([
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
])

let field2 = []
for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
        let num;
        num = Number(String(j) + String(i))
        field2.push(String(num)[0] === '0' && num != 0 ? num[1] : num)
    }
}

function allCombs(length) {
    let combs = []
    for (let i = 0; i < field.value.length; i++) {
        let comb = []
        for (let j = 0; j < length; j++) {
            comb.push(i + j)
        }
        let firstNum = String(comb[0])[0]
        if (comb.every(e => String(e).length === 1) || comb.every(e => String(e)[0] === firstNum)) {
            combs.push(comb)
        }
    }

    for (let i of field2) {
        if (length === 1) {
            break
        }
        let comb = []
        for (let j = 0; j < length; j++) {
            comb.push(i + j * 10)
        }
        if (comb.every(e => String(e).length <= 2)) {
            combs.push(comb)
        }
    }
    return combs
}

for (let comb of allCombs(2)) {
    if (comb.every(e => field.value[e] !== 1 && field.value[e] !== '*' && !check(e))) {
        comb.map(e => probs.value[e] += 1)
    }
}
console.log(probs.value)

function setField(cell) {
    field.value[cell-1] = field.value[cell-1] === 1 || field.value[cell-1] === 'x' ? 'x' : '*'
}

for (let cell = 0; cell < field.length; cell++) {
    // let lastNum = Number(String(cell)[String(cell).length-1])
    // if (lastNum === 0) {
    //     if (cell === 0) {
    //         // bottom, right, bottom-right
    //     } else if (cell === 90) {
    //         // top, right, top-right
    //     } else {
    //         // top, right, bottom, top-right, bottom-right
    //     }
    // } else if (lastNum === 9) {
    //     if (cell === 9) {
    //         // bottom, left, bottom-left
    //     } else if (cell === 99) {
    //         // top, left, top-left
    //     } else {
    //         // top, bottom, left, top-left, bottom-left
    //     }
    // } else if (String(cell).length === 1) {
    //     if (cell !== 0 && cell !== 9) {
    //         // right, left, bottom, bottom-right, bottom-left
    //     }
    // } else if (String(cell)[0] === 9 && cell !== 9) {
    //     if (cell !== 90 && cell !== 99) {
    //         // top, right, left, top-right, top-left
    //     }
    // } else {
    //     // all
    // }
}

function check(cell) {
    return checkTop(cell) || checkBottom(cell) || checkRight(cell) || checkLeft(cell) || checkTopRight(cell) || checkTopLeft(cell) || checkBottomRight(cell) || checkBottomLeft(cell)
}

function checkTop(cell) {
    if (Number(String(cell).length) !== 1) {
        return field.value[cell-10] === 1
    }
}

function checkBottom(cell) {
    if (Number(String(cell)[0]) !== 9 || cell === 9) {
        return field.value[cell+10] === 1
    }
}

function checkRight(cell) {
    if (Number(String(cell)[String(cell).length-1]) !== 9) {
        return field.value[cell+1] === 1
    }
}

function checkLeft(cell) {
    if (Number(String(cell)[String(cell).length-1]) !== 0) {
        return field.value[cell-1] === 1
    }
}

function checkTopRight(cell) {
    if (Number(String(cell).length) !== 1 && Number(String(cell)[String(cell).length-1]) !== 9) {
        return field.value[cell-9] === 1
    }
}

function checkTopLeft(cell) {
    if (Number(String(cell).length) !== 1 && Number(String(cell)[String(cell).length-1]) !== 0) {
        return field.value[cell-11] === 1
    }
}

function checkBottomRight(cell) {
    if (Number(String(cell)[0]) !== 9 && Number(String(cell)[String(cell).length-1]) !== 9) {
        return field.value[cell+11] === 1
    }
}

function checkBottomLeft(cell) {
    if (Number(String(cell)[0]) !== 9 && cell !== 9 && Number(String(cell)[String(cell).length-1]) !== 0) {
        return field.value[cell+9] === 1
    }
}

// function placeShips() {
//     for (let ship of ships) {
//         let start = randomStart()
//         let direction = randomCell(1, 4)
//         let ship = createShip(start, direction, ship.length)
//         console.log(ship)
//         while (!(ship.all(c => isFreeCell(c))) && ship.all(c => c >= 0)) {
//             direction = randomCell(1, 4)
//             ship = createShip(start, direction, ship.length)
//         }
//         ships[ship].coords = ship
//         for (s of ship) {
//             field.value[s] = 1
//         }
//     }
// }

// function randomStart() {
//     let start = randomCell(0, 99)
//     // while (!isFreeCell(start)) {
//     //     start = randomCell(0, 99)
//     // }
//     return start
// }

// function createShip(start, direction, length) {
//     let ship = []
//     for (let i = 0; i < length; i++) {
//         switch (direction) {
//             case 1:
//                 ship.push(start - 10 * i)
//                 break
//             case 2:
//                 ship.push(start + i)
//                 break
//             case 3:
//                 ship.push(start + 10 * i)
//                 break
//             case 4:
//                 ship.push(start - i)
//                 break
//         }
//     }
//     return ship
// }

// function isFreeCell(cell) {
//     console.log(cell)
//     return (field.value[cell - 1] === 0 || cell === 0) && field.value[cell + 1] === 0 && field.value[cell - 10] === 0 && field.value[cell + 10] === 0 && field.value[cell + 11] === 0 && field.value[cell - 11] === 0 && field.value[cell - 9] === 0 && field.value[cell + 9] === 0 ? true : false
// }

// function randomCell(min, max) {
//     // случайное число от min до (max+1)
//     let rand = min + Math.random() * (max + 1 - min);
//     return Math.floor(rand);
// }

</script>

<template>
  <div class="flex flex-row border-[1px] border-black">
    <div class="flex flex-col h-[500px] w-[calc(500px/11)]">
      <div class="cell cell-name"></div>
      <div v-for="cell of 10" :key="cell" class="cell cell-name">{{ cell }}</div>
    </div>
    <div class="grid grid-rows-11 grid-cols-10 h-[500px] w-[calc(500px-500px/11)]">
      <div v-for="cell of 10" :key="cell" class="cell cell-name">{{ cellsNames[cell] }}</div>
      <div v-for="cell of 100" :key="cell" class="cell" @click="setField(cell)">{{ field[cell-1] ? field[cell-1] === 1 ? 'O' : field[cell-1] : '' }}</div>
    </div>
</div>
<div class="grid grid-cols-10 grid-rows-10 gap-[5px] text-center">
    <div v-for="prob of probs" :key="prob" class="">{{ prob }}</div>
</div>
</template>