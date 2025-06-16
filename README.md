
import { useState, useEffect } from 'react';

export default function App() {
  const [currentPage, setCurrentPage] = useState('home');
  const [user, setUser] = useState(null);

  // Simuler des données
  const modules = [
    { id: 1, title: "Introduction à la rhétorique", description: "Définitions et bases historiques." },
    { id: 2, title: "Figures de style", description: "Métonymie, allitération, oxymore, etc." },
    { id: 3, title: "Techniques d'argumentation", description: "Logos, pathos, ethos." },
    { id: 4, title: "Discours célèbres", description: "Analyse de discours marquants." }
  ];

  const exercises = [
    { id: 1, question: "Quelle figure de style est utilisée dans cette phrase : 'La mer murmure ses secrets' ?", options: ['Allitération', 'Personnification', 'Métaphore'], answer: 'Personnification' },
    { id: 2, question: "Que signifie 'logos' dans le triangle argumentatif ?", options: ['Émotion', 'Éthique', 'Raison'], answer: 'Raison' }
  ];

  const resources = [
    { id: 1, title: "PDF - Figures de style", link: "#" },
    { id: 2, title: "Vidéo - Comment structurer un discours", link: "#" },
    { id: 3, title: "Texte - Discours de Roosevelt", link: "#" }
  ];

  const handleLogin = (email) => {
    const fakeUser = { email };
    setUser(fakeUser);
    localStorage.setItem('user', JSON.stringify(fakeUser));
  };

  const handleLogout = () => {
    setUser(null);
    localStorage.removeItem('user');
  };

  useEffect(() => {
    const storedUser = localStorage.getItem('user');
    if (storedUser) {
      setUser(JSON.parse(storedUser));
    }
  }, []);

  return (
    <div className="min-h-screen bg-gray-50 text-gray-800">
      {/* Header */}
      <header className="bg-white shadow-md p-4 flex justify-between items-center sticky top-0 z-10">
        <h1 className="text-2xl font-bold text-indigo-600">Rhétorique Academy</h1>
        <nav className="space-x-4">
          <button onClick={() => setCurrentPage('home')} className="hover:text-indigo-600">Accueil</button>
          <button onClick={() => setCurrentPage('courses')} className="hover:text-indigo-600">Cours</button>
          <button onClick={() => setCurrentPage('exercises')} className="hover:text-indigo-600">Exercices</button>
          <button onClick={() => setCurrentPage('resources')} className="hover:text-indigo-600">Ressources</button>
          {user ? (
            <>
              <span className="text-sm text-gray-500">Bonjour, {user.email}</span>
              <button onClick={handleLogout} className="text-sm hover:text-red-500">Se déconnecter</button>
            </>
          ) : (
            <button onClick={() => setCurrentPage('login')} className="hover:text-indigo-600">Connexion</button>
          )}
        </nav>
      </header>

      {/* Main Content */}
      <main className="container mx-auto p-6">

        {/* Home Page */}
        {currentPage === 'home' && (
          <section className="text-center py-12">
            <h2 className="text-4xl font-extrabold mb-4 text-gray-900">Apprenez la Rhétorique</h2>
            <p className="text-lg text-gray-600 max-w-2xl mx-auto mb-8">
              Maîtrisez les techniques de communication persuasive, explorez des figures de style et analysez des discours mémorables.
            </p>
            <img src="https://picsum.photos/800/400"  alt="Rhétorique" className="mx-auto rounded shadow-lg mb-8" />
            <div className="flex justify-center space-x-4">
              <button onClick={() => setCurrentPage('courses')} className="bg-indigo-600 text-white px-6 py-3 rounded hover:bg-indigo-700 transition">
                Commencer les cours
              </button>
              <button onClick={() => setCurrentPage('exercises')} className="bg-white border border-indigo-600 text-indigo-600 px-6 py-3 rounded hover:bg-indigo-50 transition">
                Faire un exercice
              </button>
            </div>
          </section>
        )}

        {/* Courses */}
        {currentPage === 'courses' && (
          <section>
            <h2 className="text-3xl font-bold mb-6">Modules de Cours</h2>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              {modules.map((module) => (
                <div key={module.id} className="bg-white p-6 rounded shadow hover:shadow-lg transition">
                  <h3 className="text-xl font-semibold mb-2">{module.title}</h3>
                  <p className="text-gray-600">{module.description}</p>
                </div>
              ))}
            </div>
          </section>
        )}

        {/* Exercises */}
        {currentPage === 'exercises' && (
          <section>
            <h2 className="text-3xl font-bold mb-6">Exercices Interactifs</h2>
            {exercises.map((exercise) => (
              <ExerciseCard key={exercise.id} exercise={exercise} />
            ))}
          </section>
        )}

        {/* Resources */}
        {currentPage === 'resources' && (
          <section>
            <h2 className="text-3xl font-bold mb-6">Ressources Téléchargeables</h2>
            <ul className="space-y-4">
              {resources.map((resource) => (
                <li key={resource.id}>
                  <a href={resource.link} className="block p-4 bg-white rounded shadow hover:shadow-md transition">
                    {resource.title}
                  </a>
                </li>
              ))}
            </ul>
          </section>
        )}

        {/* Login */}
        {currentPage === 'login' && (
          <section className="max-w-md mx-auto mt-12">
            <h2 className="text-3xl font-bold mb-6 text-center">Se connecter</h2>
            <form onSubmit={(e) => {
              e.preventDefault();
              const email = e.target.email.value;
              handleLogin(email);
              setCurrentPage('home');
            }} className="bg-white p-6 rounded shadow">
              <div className="mb-4">
                <label htmlFor="email" className="block text-sm font-medium text-gray-700 mb-1">Email</label>
                <input type="email" id="email" name="email" required className="w-full px-4 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-indigo-300" />
              </div>
              <button type="submit" className="w-full bg-indigo-600 text-white py-2 rounded hover:bg-indigo-700 transition">
                Se connecter
              </button>
            </form>
          </section>
        )}

      </main>

      {/* Footer */}
      <footer className="bg-white shadow-inner mt-12 py-6 text-center text-sm text-gray-500">
        © 2025 Rhétorique Academy. Tous droits réservés.
      </footer>
    </div>
  );
}

// Composant Exercice
function ExerciseCard({ exercise }) {
  const [selectedOption, setSelectedOption] = useState('');
  const [showResult, setShowResult] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();
    setShowResult(true);
  };

  return (
    <div className="bg-white p-6 rounded shadow mb-6">
      <h3 className="font-semibold text-lg mb-4">{exercise.question}</h3>
      <form onSubmit={handleSubmit}>
        {exercise.options.map((option, index) => (
          <div key={index} className="mb-2">
            <label className="inline-flex items-center">
              <input
                type="radio"
                name={`q${exercise.id}`}
                value={option}
                onChange={(e) => setSelectedOption(e.target.value)}
                className="mr-2"
              />
              <span>{option}</span>
            </label>
          </div>
        ))}
        <button type="submit" className="mt-4 bg-indigo-600 text-white px-4 py-2 rounded hover:bg-indigo-700 transition">
          Valider
        </button>
      </form>
      {showResult && (
        <div className={`mt-4 p-3 rounded ${selectedOption === exercise.answer ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}`}>
          {selectedOption === exercise.answer ? '✅ Bonne réponse !' : `❌ Mauvaise réponse. La bonne réponse est : ${exercise.answer}.`}
        </div>
      )}
    </div>
  );
}
