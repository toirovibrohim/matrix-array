# matrix-array

```let array = [[1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
    [13, 14, 15, 16]]


function matrix(arr) {
    const loopTimes = arr.length * 2 - 1;
    let options = {line: true, start: true, count: 0};
    let result = [];

    for (let i = 0; i < loopTimes; i++) {
        let {updatedOptions, updatedResult} = loop(options, arr);
        options = updatedOptions;
        updatedResult = updatedResult.filter(number => !result.includes(number));
        result.push(...updatedResult);
    }
    return result;
}


function loop(opt, arr) {
    let result = [];
    let startPoint = opt.count / 4;
    startPoint = Math.floor(startPoint);
    opt.count++;

    if (opt.line && opt.start) {
        // looping top side of array
        opt.line = !opt.line;
        arr[startPoint].forEach(n => result.push(n));
    } else if (opt.line && !opt.start) {
        // looping bottom of array
        opt.line = !opt.line;
        const startLoop = arr.length - 1 - startPoint;
        arr[startLoop].forEach(n => result.push(n));
        result = result.reverse();
    } else if (!opt.line && opt.start) {
        // looping right side of array
        const {r, o} = loopSides(startPoint, opt, arr);
        result.push(...r);
        opt = o;
    } else if (!opt.line && !opt.start) {
        // looping left side of array
        let {r, o} = loopSides(startPoint, opt, arr);
        r = r.reverse();
        result.push(...r);
        opt = o;
    }
    return { updatedOptions: opt, updatedResult: result };
}

function loopSides(startPoint, options, arr) {
    const result = [];
    const side = !options.line && !options.start ? 'left' : 'right';
    options.line = !options.line;
    options.start = !options.start;
    const startLoop = arr.length - 1 - startPoint;

    for (let i = 0; i < arr.length; i++) {
        result.push(arr[i][side === 'right' ? startLoop : startPoint]);
    }

    return {r: result, o: options};
}

const result = matrix(array);
// expected output: 1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10
```
