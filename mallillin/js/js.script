 //
        // JAVASCRIPT FOR DARK MODE, SCROLLSPY, TYPEWRITER, AND CUSTOM CURSOR
        //

        // --- Typewriter Logic ---
        var TxtType = function(el, toRotate, period) {
            this.toRotate = toRotate;
            this.el = el;
            this.loopNum = 0;
            this.period = parseInt(period, 10) || 2000;
            this.txt = '';
            this.isDeleting = false;
            this.tick();
        };

        TxtType.prototype.tick = function() {
            var i = this.loopNum % this.toRotate.length;
            var fullTxt = this.toRotate[i];

            if (this.isDeleting) {
                this.txt = fullTxt.substring(0, this.txt.length - 1);
            } else {
                this.txt = fullTxt.substring(0, this.txt.length + 1);
            }

            this.el.innerHTML = '<span class="wrap">' + this.txt + '</span>';

            var that = this;
            var delta = 200 - Math.random() * 100;

            if (this.isDeleting) { delta /= 2; }

            if (!this.isDeleting && this.txt === fullTxt) {
                delta = this.period;
                this.isDeleting = true;
            } 
            else if (this.isDeleting && this.txt === '') {
                this.isDeleting = false;
                this.loopNum++;
                delta = 500;
            }

            setTimeout(function() {
                that.tick();
            }, delta);
        };
        
        // --- Custom Cursor Logic ---
        function initializeCustomCursor() {
            const cursorSmall = document.querySelector('.cursor--small');
            const cursorBig = document.querySelector('.cursor--big');

            if (!cursorSmall || !cursorBig) return;

            let mouseX = 0;
            let mouseY = 0;
            let bigCircleX = 0;
            let bigCircleY = 0;
            const easing = 0.08; 

            function updateCursor() {
                cursorSmall.style.transform = `translate(${mouseX}px, ${mouseY}px)`;

                const dx = mouseX - bigCircleX;
                const dy = mouseY - bigCircleY;

                bigCircleX += dx * easing;
                bigCircleY += dy * easing;

                cursorBig.style.transform = `translate(${bigCircleX}px, ${bigCircleY}px)`;

                requestAnimationFrame(updateCursor);
            }

            document.body.addEventListener('mousemove', e => {
                mouseX = e.clientX;
                mouseY = e.clientY;
            });

            updateCursor();
        }

        // --- Animation Cleanup Helper ---
        function handleAnimationEnd(event) {
            // Remove the class so the animation can be re-triggered
            event.target.classList.remove('bounce-in');
            event.target.removeEventListener('animationend', handleAnimationEnd);
        }


       // --- Main DOMContentLoaded Listener ---
        document.addEventListener('DOMContentLoaded', () => {
            
            const body = document.body;
            
            // Sidebar and Toggle Elements
            const menuToggle = document.querySelector('.menu-toggle-icon'); 
            const navLinksContainer = document.querySelector('.nav-links'); // The <nav> element
            const sidebarLinks = document.querySelectorAll('.nav-links a'); // All links inside the nav
            
            // Dark Mode Element (Still inside navLinksContainer)
            const toggle = document.querySelector('.moon-icon'); 
            

            // Function to close the sidebar and reset the icon
            const closeSidebar = () => {
                 if(navLinksContainer) navLinksContainer.classList.remove('open');
                 if(menuToggle) {
                     menuToggle.classList.remove('fa-xmark');
                     menuToggle.classList.add('fa-bars');
                 }
            };

            // 1. Sidebar Toggle Logic (New/Upgraded Logic)
            if (menuToggle && navLinksContainer) {
                
                menuToggle.addEventListener('click', (e) => {
                    // Stop event from bubbling up and potentially interfering with other elements
                    e.stopPropagation(); 
                    
                    navLinksContainer.classList.toggle('open');
                    
                    if (navLinksContainer.classList.contains('open')) {
                        menuToggle.classList.remove('fa-bars');
                        menuToggle.classList.add('fa-xmark'); // Change to X icon
                    } else {
                        menuToggle.classList.remove('fa-xmark');
                        menuToggle.classList.add('fa-bars'); // Change back to bars
                    }
                });
                
                // Close sidebar when a link is clicked
                sidebarLinks.forEach(link => {
                    link.addEventListener('click', closeSidebar);
                });
                
                // FIX: Close sidebar on click outside the menu area
                document.addEventListener('click', (e) => {
                    // Check if the sidebar is open
                    if (navLinksContainer.classList.contains('open')) {
                        // Check if the click target is NOT the toggle icon AND NOT inside the nav container
                        if (e.target !== menuToggle && !navLinksContainer.contains(e.target)) {
                             closeSidebar();
                        }
                    }
                });
            }


            // 2. Dark Mode Toggle Logic
            if (localStorage.getItem('darkMode') === 'true') {
                body.classList.add('dark-mode');
                toggle.classList.remove('fa-moon');
                toggle.classList.add('fa-sun');
            }

            toggle.addEventListener('click', () => {
                body.classList.toggle('dark-mode');
                const isDarkMode = body.classList.contains('dark-mode');
                
                if (isDarkMode) {
                    toggle.classList.remove('fa-moon');
                    toggle.classList.add('fa-sun');
                } else {
                    toggle.classList.remove('fa-sun');
                    toggle.classList.add('fa-moon');
                }

                localStorage.setItem('darkMode', isDarkMode);
            });
            

            // 3. ScrollSpy for Active Link Indicator & Bounce Effect
            const desktopNavLinks = document.querySelectorAll('.nav-links a'); 
            const sections = document.querySelectorAll('section[id]'); 
            
            const options = {
                root: null, 
                rootMargin: '0px',
                threshold: 0.5 
            };

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    const id = entry.target.getAttribute('id');
                    const link = document.querySelector(`.nav-links a[href="#${id}"]`);

                    if (entry.isIntersecting) {
                        desktopNavLinks.forEach(l => l.classList.remove('active'));
                        if (link) {
                            link.classList.add('active');
                        }
                        
                        // --- BOUNCE EFFECT LOGIC ---
                        const h2 = entry.target.querySelector('h2');
                        if (h2 && !h2.classList.contains('bounce-in')) {
                            // Apply the bounce animation class
                            h2.classList.add('bounce-in'); 
                            
                            // Remove the class after the animation finishes
                            h2.addEventListener('animationend', handleAnimationEnd);
                        }
                        // --- END BOUNCE LOGIC ---

                    }
                    if (window.scrollY < 100) {
                         desktopNavLinks.forEach(l => l.classList.remove('active'));
                    }
                });
            }, options);

            sections.forEach(section => {
                observer.observe(section);
            });


            // 4. Typewriter Initialization
            var elements = document.getElementsByClassName('typewrite');
            for (var i=0; i<elements.length; i++) {
                var toRotate = elements[i].getAttribute('data-type');
                var period = elements[i].getAttribute('data-period');
                if (toRotate) {
                    new TxtType(elements[i], JSON.parse(toRotate), period);
                }
            }
            
            // 5. Custom Cursor Initialization
            if (window.innerWidth > 768) {
                initializeCustomCursor();
            }
            
        });
