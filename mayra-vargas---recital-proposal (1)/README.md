# Mayra Vargas - Recital Proposal Website

This repository contains the source code for a single-page web application presenting an artistic proposal for a classical music recital. The website is designed to be elegant, modern, and dynamic, reflecting the high quality of the artist. It is built using React, TypeScript, and Tailwind CSS, loaded via CDNs for a simple, build-free setup.

## Project Files

The entire application is contained in the following four files. Below is the full source code for each file, consolidated for easy viewing.

---

### `index.html`

The main HTML entry point. It sets up the page structure, loads Tailwind CSS and custom fonts, defines the import map for React, and creates the root element for the application.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mayra Vargas - Recital Proposal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600;700&family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
      /* Simple scroll-bar styling for a more elegant look */
      ::-webkit-scrollbar {
        width: 8px;
      }
      ::-webkit-scrollbar-track {
        background: #1a2a2a; 
      }
      ::-webkit-scrollbar-thumb {
        background: #c5a565;
        border-radius: 4px;
      }
      ::-webkit-scrollbar-thumb:hover {
        background: #d4b880;
      }
    </style>
    <script>
      tailwind.config = {
        theme: {
          extend: {
            fontFamily: {
              'inter': ['Inter', 'sans-serif'],
              'cormorant': ['"Cormorant Garamond"', 'serif'],
            },
            colors: {
              'brand-dark': '#1a2a2a',
              'brand-gold': '#c5a565',
              'brand-light': '#e0e0e0',
            }
          },
        },
      }
    </script>
  <script type="importmap">
{
  "imports": {
    "react-dom/": "https://esm.sh/react-dom@^19.1.0/",
    "react/": "https://esm.sh/react@^19.1.0/",
    "react": "https://esm.sh/react@^19.1.0"
  }
}
</script>
</head>
  <body class="bg-brand-dark">
    <div id="root"></div>
    <script type="module" src="/index.tsx"></script>
  </body>
</html>
```

---

### `App.tsx`

This is the main React component that renders the entire user interface. It includes the layout, all content sections, animations, and helper components for the website.

```tsx
import React, { useState, useEffect, useRef } from 'react';

// --- HELPER HOOK for ANIMATIONS ---
const useIntersectionObserver = (options: IntersectionObserverInit) => {
    const [entry, setEntry] = useState<IntersectionObserverEntry | null>(null);
    const [node, setNode] = useState<HTMLElement | null>(null);

    const observer = useRef<IntersectionObserver | null>(null);

    useEffect(() => {
        if (observer.current) observer.current.disconnect();

        observer.current = new window.IntersectionObserver(([entry]) => {
            if (entry.isIntersecting) {
                setEntry(entry);
                if (node && observer.current) {
                    observer.current.unobserve(node);
                }
            }
        }, options);

        const { current: currentObserver } = observer;
        if (node) currentObserver.observe(node);

        return () => {
            if(currentObserver) currentObserver.disconnect();
        };
    }, [node, options]);

    return [setNode, entry];
};


// --- UI COMPONENTS ---

const AnimatedSection = ({ children }: { children: React.ReactNode }) => {
    const [setNode, entry] = useIntersectionObserver({ threshold: 0.2 });
    const isVisible = !!entry;

    return (
        <div
            ref={setNode as React.Ref<HTMLDivElement>}
            className={`transition-all duration-1000 transform ${isVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}`}
        >
            {children}
        </div>
    );
};

const AnimatedDivider = () => {
    const [setNode, entry] = useIntersectionObserver({ threshold: 0.5 });
    const isVisible = !!entry;
    return(
        <div ref={setNode as React.Ref<HTMLDivElement>} className="h-24 flex items-center justify-center">
             <div className="h-px bg-brand-gold transition-all duration-1000" style={{ width: isVisible ? '50%' : '0' }}></div>
        </div>
    )
};

const PhoneIcon = ({ className }: { className?: string }) => (
    <svg xmlns="http://www.w3.org/2000/svg" className={className} viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path fillRule="evenodd" d="M2 3.5A1.5 1.5 0 013.5 2h1.148a1.5 1.5 0 011.465 1.175l.716 3.578a1.5 1.5 0 01-1.052 1.767l-1.262.632a11.035 11.035 0 005.455 5.455l.632-1.262a1.5 1.5 0 011.767-1.052l3.578.716A1.5 1.5 0 0118 15.352V16.5a1.5 1.5 0 01-1.5 1.5H15c-5.523 0-10-4.477-10-10V3.5z" clipRule="evenodd" />
    </svg>
);

const MailIcon = ({ className }: { className?: string }) => (
    <svg xmlns="http://www.w3.org/2000/svg" className={className} viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
        <path d="M3 4a2 2 0 00-2 2v1.161l8.441 4.221a1.25 1.25 0 001.118 0L19 7.162V6a2 2 0 00-2-2H3z" />
        <path d="M19 8.839l-7.77 3.885a2.75 2.75 0 01-2.46 0L1 8.839V14a2 2 0 002 2h14a2 2 0 002-2V8.839z" />
    </svg>
);


// --- MAIN APP ---

const App = () => {
    const logoUrl = "https://i.ibb.co/6wmYp3G/mayra-vargas-logo-green.png";
    const photoUrl = "https://i.ibb.co/b3j4Y4K/mayra-vargas-photo.jpg";


    return (
        <div className="bg-brand-dark text-brand-light min-h-screen font-inter antialiased">
            
            {/* --- HERO SECTION --- */}
            <header className="h-screen flex flex-col items-center justify-center text-center p-4">
                 <AnimatedSection>
                    <img src={logoUrl} alt="Mayra Vargas Logo" className="mx-auto w-48 h-48 md:w-56 md:h-56 object-contain" />
                    <h1 className="text-5xl md:text-7xl font-bold text-brand-gold mt-6 font-cormorant tracking-wider">Mayra Vargas</h1>
                    <p className="text-2xl md:text-3xl text-white/80 tracking-[0.3em] uppercase mt-2">Soprano</p>
                </AnimatedSection>
                <div className="absolute bottom-10 animate-bounce">
                    <svg className="w-8 h-8 text-brand-gold" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19 9l-7 7-7-7"></path></svg>
                </div>
            </header>

            <main className="max-w-5xl mx-auto px-6 lg:px-8 py-20">

                {/* --- PROPOSAL INTRO --- */}
                <AnimatedSection>
                    <section className="text-center mb-20">
                        <h2 className="text-4xl md:text-5xl font-cormorant text-brand-gold mb-4">Propuesta de recital: Entre la risa y la promesa</h2>
                        <p className="text-white/70 text-lg">Bucaramanga, 25 de julio de 2025</p>
                    </section>
                </AnimatedSection>
                
                {/* --- ARTIST INTRODUCTION with IMAGE --- */}
                <AnimatedSection>
                    <section className="grid md:grid-cols-5 gap-12 lg:gap-16 items-center mb-16 text-lg leading-relaxed">
                        {/* Image Column */}
                        <div className="md:col-span-2">
                             <img 
                                src={photoUrl} 
                                alt="Soprano Mayra Vargas" 
                                className="rounded-lg shadow-2xl object-cover w-full h-auto aspect-[3/4]"
                            />
                        </div>
                        {/* Text Column */}
                        <div className="md:col-span-3 space-y-6">
                            <div className="text-2xl font-cormorant text-white/90">
                                <p>Señores(as)<br /><span className="text-brand-gold">Fundación Septum</span><br />Cordial saludo,</p>
                            </div>
                            <div className="space-y-4 text-white/80">
                                <p>Es un gusto dirigirme a ustedes para presentar una propuesta artística especialmente concebida para el valioso escenario que ustedes representan, reconocido por su calidez, sensibilidad y compromiso con la difusión de la cultura.</p>
                                <p>Les propongo realizar el recital titulado <span className="font-semibold text-brand-gold">“Entre la risa y la promesa”</span>, una experiencia íntima y emotiva que une repertorio vocal de cámara con textos poéticos cuidadosamente seleccionados. El recital integra a una soprano, un pianista y un narrador en escena, creando un diálogo entre música y literatura que busca conmover y conectar profundamente con el público.</p>
                            </div>
                        </div>
                    </section>
                </AnimatedSection>

                <AnimatedDivider />

                {/* --- MUSICAL PROGRAM --- */}
                <section id="program" className="mb-12">
                    <AnimatedSection>
                        <h2 className="text-4xl md:text-5xl font-semibold text-brand-gold mb-12 text-center font-cormorant">Programa Musical</h2>
                    </AnimatedSection>
                    <div className="space-y-12">
                        <AnimatedSection>
                            <ProgramItem composer="Franz Liszt" details={["Piano solo"]} />
                        </AnimatedSection>
                        <AnimatedSection>
                            <ProgramItem composer="Francis Poulenc" title="Fiançailles pour rire" details={["(Ciclo completo sobre textos de Louise de Vilmorin)", "Voz y piano"]} pieces={["I. La dame d'André", "II. Dans L'herbe", "III. Il vole", "V. Violon", "VI. Fleurs"]} />
                        </AnimatedSection>
                        <AnimatedSection>
                            <ProgramItem composer="Antonín Dvořák" title="Gypsy Songs Op. 55" details={["Voz y piano"]} pieces={["I Mein lied ertönt", "II Ei, wie mein Triangel", "IV. Als die alte Mutter", "V. Reingestimmt die Saiten"]} />
                        </AnimatedSection>
                        
                        <AnimatedSection>
                            <div className="text-center my-8">
                                <p className="text-brand-gold font-cormorant text-3xl tracking-widest border-y border-brand-gold/30 py-4 inline-block">RECESO</p>
                            </div>
                        </AnimatedSection>

                        <AnimatedSection>
                             <ProgramItem composer="Scriabin" details={["Piano solo"]} />
                        </AnimatedSection>
                        <AnimatedSection>
                            <ProgramItem composer="Ernestina Lecuona" title="Canción de cámara" details={["Voz y piano"]} pieces={["I. Cuando duermes", "II. Noches de amor"]} />
                        </AnimatedSection>
                        <AnimatedSection>
                            <ProgramItem composer="Ariel Ramírez" title="Canción de cámara" details={["(Poesía de Félix Luna)", "Voz y piano"]} pieces={["I. Alfonsina y el mar"]} />
                        </AnimatedSection>
                        <AnimatedSection>
                             <ProgramItem composer="Ernestina Lecuona" title="Canción de cámara" details={["Voz y piano"]} pieces={["I. Te quiero dijiste", "II. Júrame"]} />
                        </AnimatedSection>
                    </div>
                </section>
                
                <AnimatedDivider />

                {/* --- OTHER SECTIONS --- */}
                <SectionLayout title="Propuesta Escénica">
                    <div className="text-lg leading-relaxed space-y-6 text-white/80">
                        <p>Entre los bloques musicales, un narrador masculino recitará breves fragmentos de textos de autores como Paul Éluard, Colette, Rilke, Alfonsina Storni, Cortázar, entre otros. Esta curaduría literaria ha sido diseñada para enriquecer el discurso emocional y temático del recital, hilando las obras con belleza, sentido y sensibilidad.</p>
                        <div className="mt-8 pt-6 border-t border-brand-gold/30 text-white/90">
                            <p><span className="font-bold text-brand-gold">Duración total:</span> 55 a 65 minutos (incluye receso de 10 min).</p>
                            <p><span className="font-bold text-brand-gold">Requerimientos técnicos mínimos:</span> piano acústico, iluminación básica, y condiciones acústicas adecuadas (no requiere escenografía).</p>
                        </div>
                    </div>
                </SectionLayout>

                <SectionLayout title="Elenco Artístico">
                     <div className="text-lg leading-relaxed text-white/80">
                        <p className="mb-6">Artistas colombianos egresados del programa de música de la UNAB y ganadores del programa Jóvenes Intérpretes 2025 del Banco de la República:</p>
                        <ul className="list-none space-y-3">
                           <li><span className="font-bold text-white text-xl">Mayra Vargas</span> – <span className="text-brand-gold">Soprano</span></li>
                           <li><span className="font-bold text-white text-xl">Alfredo Saad</span> – <span className="text-brand-gold">Piano</span></li>
                           <li><span className="font-bold text-white text-xl">Daniel Valderrama</span> – <span className="text-brand-gold">Narrador en escena</span></li>
                        </ul>
                    </div>
                </SectionLayout>

                <SectionLayout title="Propuesta de Coproducción">
                      <div className="text-lg leading-relaxed space-y-4 text-white/80">
                        <p>Proponemos un modelo de coproducción en los siguientes términos:</p>
                        <ul className="list-none space-y-4 pl-4 my-6 border-l-2 border-brand-gold/50">
                            <li>Fundación Septum recibiría el <span className="font-bold text-white">30% de la boletería</span> y el <span className="font-bold text-white">100% de las ventas de cafetería</span> (vino, café u otras bebidas).</li>
                            <li>El equipo artístico recibiría el <span className="font-bold text-white">70% restante de la boletería</span>.</li>
                        </ul>
                        <p>Creemos que esta fórmula permite una colaboración sostenible, transparente y mutuamente enriquecedora, que potencia tanto la programación del espacio como el desarrollo artístico.</p>
                    </div>
                </SectionLayout>
                
                <AnimatedDivider />

                {/* --- FOOTER --- */}
                <footer className="text-center mt-16 pt-8">
                    <AnimatedSection>
                        <div className="text-lg leading-relaxed space-y-4 max-w-3xl mx-auto text-white/80">
                             <p>Estoy convencida de que <span className="font-semibold text-brand-gold">Entre la risa y la promesa</span> enriquecerá la oferta cultural de su espacio y resonará en aquellos públicos que valoran la belleza, la palabra y la experiencia compartida.</p>
                             <p>Quedo atenta a su respuesta para coordinar detalles administrativos y definir fechas posibles para su realización. Agradezco de antemano su atención e interés.</p>
                        </div>

                        <div className="mt-16">
                            <p className="font-cormorant text-2xl text-white/90">Con aprecio,</p>
                            <p className="font-cormorant text-4xl font-semibold text-brand-gold mt-1">Mayra Vargas</p>
                            <p className="text-lg text-white/80">Soprano colombiana</p>
                        </div>
                    </AnimatedSection>

                    <AnimatedSection>
                        <div className="mt-8 flex flex-col sm:flex-row justify-center items-center gap-x-8 gap-y-4 text-lg text-white/90">
                            <div className="flex items-center gap-2">
                                <PhoneIcon className="w-5 h-5 text-brand-gold" />
                                <span>318 416 6362</span>
                            </div>
                            <a href="mailto:sopranomayravargas@gmail.com" className="flex items-center gap-2 hover:text-brand-gold transition-colors duration-300">
                                <MailIcon className="w-5 h-5 text-brand-gold" />
                                <span>sopranomayravargas@gmail.com</span>
                            </a>
                        </div>
                    </AnimatedSection>
                    
                    <AnimatedSection>
                        <div className="mt-20">
                            <img src={logoUrl} alt="Mayra Vargas Logo" className="mx-auto w-28 h-28 object-contain opacity-80" />
                        </div>
                    </AnimatedSection>
                </footer>
            </main>
        </div>
    );
};


// --- REUSABLE LAYOUT & ITEM COMPONENTS ---

const SectionLayout = ({ title, children }: { title: string, children: React.ReactNode }) => (
    <AnimatedSection>
        <section className="my-20 grid md:grid-cols-12 gap-8">
            <div className="md:col-span-4">
                <h2 className="text-3xl md:text-4xl font-semibold text-brand-gold font-cormorant sticky top-20">{title}</h2>
            </div>
            <div className="md:col-span-8">
                {children}
            </div>
        </section>
    </AnimatedSection>
);

const ProgramItem = ({ composer, title, details, pieces }: { composer: string, title?: string, details: string[], pieces?: string[] }) => (
    <div className="border-l-2 border-brand-gold/30 pl-6 py-2">
        <h3 className="text-3xl font-cormorant font-semibold text-brand-gold">{composer}</h3>
        {title && <p className="text-2xl font-cormorant text-white/80">{title}</p>}
        {details.map((detail, index) => (
            <p key={index} className="italic text-brand-gold/80 text-sm mb-2">{detail}</p>
        ))}
        {pieces && (
            <ul className="list-none pl-6 mt-4 grid grid-cols-1 sm:grid-cols-2 gap-x-6 gap-y-2 text-white/80">
                {pieces.map((piece, index) => (
                    <li key={index} className="mb-1">{piece}</li>
                ))}
            </ul>
        )}
    </div>
);


export default App;
```

---

### `index.tsx`

The entry point for the React application. It finds the root DOM element and uses `ReactDOM.createRoot` to render the `App` component.

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const rootElement = document.getElementById('root');
if (!rootElement) {
  throw new Error("Could not find root element to mount to");
}

const root = ReactDOM.createRoot(rootElement);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

### `metadata.json`

This file contains basic metadata for the application, such as its name and description.

```json
{
  "name": "Mayra Vargas - Recital Proposal",
  "description": "A web-based proposal for a classical music recital titled 'Entre la risa y la promesa', featuring soprano Mayra Vargas."
}
```

## How to Upload to GitHub

1.  Create a new repository on your GitHub account.
2.  On your computer, place the four files (`README.md`, `index.html`, `App.tsx`, `index.tsx`, and `metadata.json`) into a new folder.
3.  Follow GitHub's instructions to upload the files from that folder into your new repository.

Once uploaded, this `README.md` file will be displayed as the homepage of your repository.
