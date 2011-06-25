Classic starfield
=================

Simple perspective projection.

<canvas id="canvas-v2" width="500" height="200"></canvas>

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js" type="text/javascript"></script>
<script type="application/javascript">
$(function () {
    function Starfield () {
        var w = 500, h = 200, d = 300;
        var stars = [];

        function createStars(n) {
            for (var i = 0; i < n; ++i) {
                stars.push({
                    x: Math.random() * 500 - 250,
                    y: Math.random() * 500 - 250,
                    z: Math.random() * 500 - 250
                });
            }
        }

        function dot(img, x, y, col) {
            x = Math.round(x);
            y = Math.round(y);
            if (x < 0 || x >= img.width) return;
            if (y < 0 || y >= img.height) return;
            var base = img.width*4*Math.round(y) + 4*Math.round(x);
            img.data[base + 0] = col[0];
            img.data[base + 1] = col[1];
            img.data[base + 2] = col[2];
            img.data[base + 3] = col[3];
        }

        function colorFromDepth(z) {
            var d = 1-z;
            return [64+96*d, 64+96*d, 64+128*d, 255];
        }

        function drawStars(img) {
            for (var i = 0, l = stars.length; i < l; ++i) {
                var z = stars[i].z / 250;
                if (z <= 0.1 || z > 1) continue;
                var xm = stars[i].x / z;
                var ym = stars[i].y / z;
                dot(img, xm + w/2, ym + h/2, colorFromDepth(z));
            }
        }

        function step() {
            for (var i = 0, l = stars.length; i < l; ++i) {
                stars[i].z -= 0.5;
                if (stars[i].z <= -250) {
                    stars[i].z += 500;
                }
            }
        }

        function draw(ctx) {
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, w, h);
            var img = ctx.getImageData(0, 0, w, h);
            drawStars(img);
            ctx.putImageData(img, 0, 0, 0, 0, w, h);
        }


        createStars(1000);

        return {
            step: step,
            draw: draw
        };
    };

    var ctx = $("#canvas-v2")[0].getContext("2d");

    var starfield = Starfield();

    setInterval(function () {
        starfield.step();
        starfield.draw(ctx);
    }, 10);
});
</script>
