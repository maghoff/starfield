Simple scroller
===============

No perspective.

<canvas id="canvas-v1" width="500" height="200"></canvas>

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js" type="text/javascript"></script>
<script type="application/javascript">
$(function () {
    function Starfield () {
        var w = 500, h = 200, d = 300;
        var stars = [];

        function createStars(n) {
            for (var i = 0; i < n; ++i) {
                stars.push({
                    x: Math.random() * w,
                    y: Math.random() * h,
                    z: Math.random() * d
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

        function colorFromDepth(w) {
            return [196*w, 196*w, 255*w, 255];
        }

        function drawStars(img) {
            for (var i = 0, l = stars.length; i < l; ++i) {
                dot(img, stars[i].x, stars[i].y, colorFromDepth(stars[i].z / 300));
            }
        }

        function step() {
            for (var i = 0, l = stars.length; i < l; ++i) {
                stars[i].x += 1;
                if (stars[i].x >= w) {
                    stars[i].x -= w;
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


        createStars(300);

        return {
            step: step,
            draw: draw
        };
    };

    var canvas = $("#canvas-v1")[0];
    var ctx = canvas.getContext("2d");

    var starfield = Starfield();

    setInterval(function () {
        starfield.step();
        starfield.draw(ctx);
    }, 100);
});
</script>
