function editCore(core) {
    core = core.replace(/;if\((\w)<1\.0\){/i, ';if($1<0){');
    core = core.replace(/([\w]+\s*=\s*[\w]+\s*\+\s*16\s*\|\s*0;\s*([\w=]+)\s*=\s*\+[\w\[\s*><\]]+;)/, '$1 $2*=0.75;');
    core = core.replace(
        /([\w$]+\(\d+,\w\[\w>>2\]\|0,(\+\w),(\+\w)\)\|0;[\w$]+\(\d+,\w\[\w>>2\]\|0,\+-(\+\w\[\w\+\d+>>3\]),\+-(\+\w\[\w\+\d+>>3\])\)\|0;)/i,
        '$1 window.viewScale=$2; if (window.coordOffsetFixed) { window.playerX=$4+window.offsetX; window.playerY=$5+window.offsetY;} if(window.draw){window.draw();}'
    );
    core = core.replace(
        /(\w\[\w\+(\d+)>>3]=(\w);\w\[\w\+(\d+)>>3]=(\w);\w\[\w\+(\d+)>>3]=(\w);\w\[\w\+(\d+)>>3]=(\w);\w\=\w\+(\d+)\|(\d+);)/i,
        '$1 function setMapCoords(_0x7e8bx1, _0x7e8bx2, _0x7e8bx3, _0x7e8bx4, _0x7e8bx5, _0x7e8bx6) { if (_0x7e8bx6 - _0x7e8bx5 == 24) { if (_0x7e8bx3 - _0x7e8bx1 > 14E3) { if (_0x7e8bx4 - _0x7e8bx2 > 14E3) { window.offsetX = 7071.067811865476 - _0x7e8bx3; window.offsetY = 7071.067811865476 - _0x7e8bx4; window.minX = _0x7e8bx1;window.minY=_0x7e8bx2;window.maxX=_0x7e8bx3;window.maxY=_0x7e8bx4; window.coordOffsetFixed = true; } } } } setMapCoords($3,$5,$7,$9,$2,$8);'
    );
    console.log('core_edited');
    return core;
}

window.draw = () => {
    if (!window.minX || !window.minY || !window.maxY || !window.maxY) return;
    const ctx = document.getElementById('canvas').getContext('2d');
    ctx.save();
    ctx.strokeStyle = '#0000ff';
    ctx.lineWidth = 20;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.beginPath();
    ctx.moveTo(window.minX, window.minY);
    ctx.lineTo(window.maxX, window.minY);
    ctx.lineTo(window.maxX, window.maxY);
    ctx.lineTo(window.minX, window.maxY);
    ctx.closePath();
    ctx.stroke();
    ctx.restore();
}

let observer = new MutationObserver((mutations) => {
    mutations.forEach((mutation) => {
        mutation.addedNodes.forEach((node) => {
            if (/agario\.core\.js/i.test(node.src)) {
                observer.disconnect();
                node.parentNode.removeChild(node);
                let request = new XMLHttpRequest();
                request.open('get', node.src, true);
                request.send();
                request.onload = function() {
                    let coretext = this.responseText;
                    let newscript = document.createElement('script');
                    newscript.type = 'text/javascript';
                    newscript.async = true;
                    newscript.textContent = editCore(coretext);
                    document.body.appendChild(newscript);
                    setTimeout(() => {
                        window.client = new Client();
                    }, 3500);
                }
            }
        });
    });
});
