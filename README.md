import React, { useState, useEffect } from 'react';
import { Mail, Phone, MapPin, Calendar, Users, ArrowRight, ChevronDown } from 'lucide-react';

// Mock Data for Pilgrimage Tours
const mockTours = [
  {
    id: 1,
    title: "Khatu Shyam Ji Yatra",
    destination: "Khatu, Rajasthan",
    date: "2025-09-15",
    vanType: "Toyota Innova",
    image: "https://i.ibb.co/ZpwgYpKp/khatu-shyam-ji-temple-sikar-rajasthan-india-wallpaper-preview.jpg",
    description: "Embark on a divine journey to the sacred temple of Khatu Shyam Ji, a revered pilgrimage site.",
    bookedSeats: ['1A', '2B', '3A'], // Example of already booked seats
  },
  {
    id: 2,
    title: "Ujjain Mahakal Darshan",
    destination: "Ujjain, Madhya Pradesh",
    date: "2025-10-05",
    vanType: "Tempo Traveller",
    image: "https://i.ibb.co/gbZwnqCM/ujjain-mahakal-darshan.jpg",
    description: "Experience the spiritual aura of the Mahakaleshwar Jyotirlinga, one of the twelve sacred jyotirlingas.",
    bookedSeats: ['1A'],
  },
  {
    id: 3,
    title: "Salasar Balaji Yatra",
    destination: "Salasar, Rajasthan",
    date: "2025-11-20",
    vanType: "Toyota Innova",
    image: "https://placehold.co/600x400/F97316/333333?text=Salasar+Balaji",
    description: "Seek blessings at the renowned Salasar Balaji temple, a place of immense faith and devotion.",
    bookedSeats: ['2A', '2C', '3B'],
  },
];

// Seat Configuration and Pricing
const SEAT_CONFIG = {
    '1A': { price: 1400, type: 'Front' }, // Front Seat
    '2A': { price: 1300, type: 'Middle' },
    '2B': { price: 1300, type: 'Middle' },
    '2C': { price: 1300, type: 'Middle' },
    '3A': { price: 1200, type: 'Back' },
    '3B': { price: 1200, type: 'Back' },
};

// Helper function to format date
const formatDate = (dateString) => {
    const options = { year: 'numeric', month: 'long', day: 'numeric' };
    return new Date(dateString).toLocaleDateString(undefined, options);
};


// --- Components ---

// Navbar Component
const Navbar = ({ setPage }) => {
    const [isOpen, setIsOpen] = useState(false);

    const navLinks = [
        { name: 'Home', page: 'home' },
        { name: 'Tours', page: 'tours' },
        { name: 'Book Now', page: 'booking' },
        { name: 'About Us', page: 'about' },
        { name: 'Contact', page: 'contact' },
    ];

    return (
        <nav className="bg-white/80 backdrop-blur-md shadow-md sticky top-0 z-50">
            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div className="flex items-center justify-between h-20">
                    <div className="flex-shrink-0">
                        <a href="#" onClick={() => setPage('home')} className="text-2xl font-bold text-amber-600">
                            Shubh Yatra
                        </a>
                    </div>
                    <div className="hidden md:block">
                        <div className="ml-10 flex items-baseline space-x-4">
                            {navLinks.map((link) => (
                                <a
                                    key={link.name}
                                    href="#"
                                    onClick={() => setPage(link.page)}
                                    className="text-gray-700 hover:bg-amber-500 hover:text-white px-3 py-2 rounded-md text-sm font-medium transition-colors duration-300"
                                >
                                    {link.name}
                                </a>
                            ))}
                        </div>
                    </div>
                    <div className="-mr-2 flex md:hidden">
                        <button
                            onClick={() => setIsOpen(!isOpen)}
                            type="button"
                            className="bg-amber-500 inline-flex items-center justify-center p-2 rounded-md text-white hover:bg-amber-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-amber-500 focus:ring-white"
                            aria-controls="mobile-menu"
                            aria-expanded="false"
                        >
                            <span className="sr-only">Open main menu</span>
                            {!isOpen ? (
                                <svg className="block h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 6h16M4 12h16M4 18h16" />
                                </svg>
                            ) : (
                                <svg className="block h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            )}
                        </button>
                    </div>
                </div>
            </div>

            {isOpen && (
                <div className="md:hidden" id="mobile-menu">
                    <div className="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                        {navLinks.map((link) => (
                             <a
                                key={link.name}
                                href="#"
                                onClick={() => { setPage(link.page); setIsOpen(false); }}
                                className="text-gray-700 hover:bg-amber-500 hover:text-white block px-3 py-2 rounded-md text-base font-medium transition-colors duration-300"
                            >
                                {link.name}
                            </a>
                        ))}
                    </div>
                </div>
            )}
        </nav>
    );
};

// Hero Component with Image Slider
const HeroSection = ({ setPage, images }) => {
    const [currentIndex, setCurrentIndex] = useState(0);

    useEffect(() => {
        if (!images || images.length <= 1) return;

        const timer = setInterval(() => {
            setCurrentIndex((prevIndex) => (prevIndex + 1) % images.length);
        }, 5000); // Change image every 5 seconds

        return () => clearInterval(timer);
    }, [images]);

    return (
        <div className="relative bg-gray-800 h-[60vh] md:h-[80vh] overflow-hidden">
            {images.map((image, index) => (
                <img
                    key={index}
                    className={`absolute inset-0 w-full h-full object-cover transition-opacity duration-1000 ease-in-out ${index === currentIndex ? 'opacity-100' : 'opacity-0'}`}
                    src={image}
                    alt={`Pilgrimage slide ${index + 1}`}
                    onError={(e) => { e.target.onerror = null; e.target.src = 'https://placehold.co/1920x1080/FBBF24/333333?text=Shubh+Yatra'; }}
                />
            ))}
            <div className="absolute inset-0 bg-black/60" aria-hidden="true"></div>
            <div className="relative max-w-7xl mx-auto py-24 px-4 sm:py-32 sm:px-6 lg:px-8 text-center flex flex-col justify-center items-center h-full">
                <h1 className="text-4xl font-extrabold tracking-tight text-white sm:text-5xl lg:text-6xl">
                    Begin Your <span className="text-amber-400">Spiritual Journey</span>
                </h1>
                <p className="mt-6 max-w-lg mx-auto text-xl text-amber-100">
                    Comfortable, safe, and blessed travels to India's most sacred destinations.
                </p>
                <div className="mt-10 max-w-sm mx-auto sm:max-w-none sm:flex sm:justify-center space-y-4 sm:space-y-0 sm:space-x-4">
                    <a
                        href="#"
                        onClick={() => setPage('tours')}
                        className="inline-block bg-amber-500 border border-transparent rounded-md py-3 px-8 font-medium text-white hover:bg-amber-600 transition-transform transform hover:scale-105"
                    >
                        Explore Tours
                    </a>
                    <a
                        href="#"
                        onClick={() => setPage('booking')}
                        className="inline-block bg-white/20 border border-transparent rounded-md py-3 px-8 font-medium text-white hover:bg-white/30 transition-transform transform hover:scale-105"
                    >
                        Book a Taxi
                    </a>
                </div>
            </div>
        </div>
    );
};

// Featured Tours for Homepage
const FeaturedTours = ({ setPage, setSelectedTour }) => (
    <div className="bg-amber-50 py-12">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div className="text-center">
                <h2 className="text-base font-semibold text-amber-600 tracking-wide uppercase">Our Yatras</h2>
                <p className="mt-2 text-3xl font-extrabold text-gray-900 tracking-tight sm:text-4xl">
                    Upcoming Pilgrimages
                </p>
            </div>
            <div className="mt-10 grid gap-8 md:grid-cols-2 lg:grid-cols-3">
                {mockTours.slice(0, 3).map((tour) => (
                    <div key={tour.id} className="flex flex-col rounded-lg shadow-lg overflow-hidden transform hover:-translate-y-2 transition-transform duration-300">
                        <div className="flex-shrink-0">
                            <img className="h-48 w-full object-cover" src={tour.image} alt={tour.title} />
                        </div>
                        <div className="flex-1 bg-white p-6 flex flex-col justify-between">
                            <div className="flex-1">
                                <p className="text-sm font-medium text-amber-600">{tour.destination}</p>
                                <a href="#" onClick={() => { setSelectedTour(tour); setPage('booking'); }} className="block mt-2">
                                    <p className="text-xl font-semibold text-gray-900">{tour.title}</p>
                                    <p className="mt-3 text-base text-gray-500">{tour.description.substring(0, 100)}...</p>
                                </a>
                            </div>
                            <div className="mt-6 flex items-center">
                                <div className="flex-shrink-0">
                                    <Calendar className="h-5 w-5 text-gray-400" />
                                </div>
                                <div className="ml-3">
                                    <p className="text-sm font-medium text-gray-900">{formatDate(tour.date)}</p>
                                </div>
                            </div>
                            <button 
                                onClick={() => { setSelectedTour(tour); setPage('booking'); }}
                                className="mt-4 w-full bg-amber-500 text-white py-2 px-4 rounded-md hover:bg-amber-600 transition-colors"
                            >
                                Book Seats
                            </button>
                        </div>
                    </div>
                ))}
            </div>
        </div>
    </div>
);

// Home Page Component
const HomePage = ({ setPage, setSelectedTour }) => {
    const heroImages = mockTours
        .map(tour => tour.image)
        .filter(image => image.startsWith('https://i.ibb.co'));

    if (heroImages.length === 0) {
        heroImages.push('https://placehold.co/1920x1080/000000/FFFFFF?text=Spiritual+Journeys');
    }
    
    return (
        <div>
            <HeroSection setPage={setPage} images={heroImages} />
            <FeaturedTours setPage={setPage} setSelectedTour={setSelectedTour} />
        </div>
    );
};

// Tours Page Component
const ToursPage = ({ setPage, setSelectedTour }) => (
    <div className="bg-gray-50 py-12">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div className="text-center">
                <h1 className="text-4xl font-extrabold text-gray-900 tracking-tight">Pilgrimage Tours</h1>
                <p className="mt-4 max-w-2xl mx-auto text-xl text-gray-500">
                    Join our organized group yatras to sacred destinations.
                </p>
            </div>
            <div className="mt-12 grid gap-10 md:grid-cols-2 lg:grid-cols-3">
                {mockTours.map((tour) => (
                    <div key={tour.id} className="bg-white rounded-lg shadow-lg overflow-hidden flex flex-col">
                        <img className="h-56 w-full object-cover" src={tour.image} alt={tour.title} />
                        <div className="p-6 flex flex-col flex-grow">
                            <h3 className="text-2xl font-semibold text-gray-900">{tour.title}</h3>
                            <div className="mt-2 flex items-center text-gray-500">
                                <MapPin className="h-5 w-5 mr-2 text-amber-500" />
                                <span>{tour.destination}</span>
                            </div>
                            <div className="mt-2 flex items-center text-gray-500">
                                <Calendar className="h-5 w-5 mr-2 text-amber-500" />
                                <span>{formatDate(tour.date)}</span>
                            </div>
                            <p className="mt-4 text-gray-600 flex-grow">{tour.description}</p>
                            <div className="mt-6">
                                <div className="flex justify-between items-center text-sm text-gray-600">
                                    <span>Van Type: {tour.vanType}</span>
                                    <span>Seats Available: {Object.keys(SEAT_CONFIG).length - tour.bookedSeats.length}</span>
                                </div>
                                <button
                                    onClick={() => { setSelectedTour(tour); setPage('booking'); }}
                                    className="mt-4 w-full bg-amber-600 text-white py-3 px-4 rounded-lg font-semibold hover:bg-amber-700 transition-all duration-300 transform hover:scale-105"
                                >
                                    View & Book Seats
                                </button>
                            </div>
                        </div>
                    </div>
                ))}
            </div>
        </div>
    </div>
);

// Van Layout for Seat Selection
const VanLayout = ({ bookedSeats, selectedSeats, onSeatSelect }) => {
    const renderSeat = (seatId) => {
        const isBooked = bookedSeats.includes(seatId);
        const isSelected = selectedSeats.includes(seatId);
        const seatInfo = SEAT_CONFIG[seatId];

        let seatClass = 'border-2 rounded-lg cursor-pointer transition-all duration-200 flex flex-col items-center justify-center p-2 text-xs font-semibold ';
        if (isBooked) {
            seatClass += 'bg-gray-400 border-gray-500 text-white cursor-not-allowed';
        } else if (isSelected) {
            seatClass += 'bg-green-500 border-green-600 text-white ring-2 ring-offset-2 ring-green-500';
        } else {
            seatClass += 'bg-amber-100 border-amber-400 hover:bg-amber-200';
        }

        return (
            <div
                key={seatId}
                onClick={() => !isBooked && onSeatSelect(seatId)}
                className={seatClass}
                style={{ aspectRatio: '1 / 1' }}
            >
                <span>{seatId}</span>
                <span className="text-[10px]">₹{seatInfo.price}</span>
            </div>
        );
    };

    return (
        <div className="bg-gray-100 p-4 rounded-lg border-2 border-dashed border-gray-300">
            <div className="max-w-xs mx-auto">
                <div className="mb-4 text-center font-bold text-gray-600">FRONT (Driver)</div>
                {/* Front Seat */}
                <div className="grid grid-cols-3 gap-2 mb-6">
                    {renderSeat('1A')}
                    <div className="col-span-2"></div> {/* Empty space */}
                </div>
                {/* Middle Seats */}
                <div className="grid grid-cols-3 gap-2 mb-6">
                    {renderSeat('2A')}
                    {renderSeat('2B')}
                    {renderSeat('2C')}
                </div>
                {/* Back Seats */}
                <div className="grid grid-cols-3 gap-2">
                    <div className="col-span-1"></div> {/* Empty space */}
                    {renderSeat('3A')}
                    {renderSeat('3B')}
                </div>
                <div className="mt-4 text-center font-bold text-gray-600">BACK</div>
            </div>
        </div>
    );
};


// Booking Page Component
const BookingPage = ({ selectedTour, setSelectedTour }) => {
    const [bookingType, setBookingType] = useState(selectedTour ? 'pilgrimage' : 'point-to-point');
    const [selectedSeats, setSelectedSeats] = useState([]);
    const [totalPrice, setTotalPrice] = useState(0);

    // Form state
    const [formData, setFormData] = useState({
        name: '',
        phone: '',
        email: '',
        from: '',
        to: '',
        date: '',
    });

    const handleInputChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({ ...prev, [name]: value }));
    };

    const handleTourChange = (e) => {
        const tourId = parseInt(e.target.value);
        const tour = mockTours.find(t => t.id === tourId);
        setSelectedTour(tour);
        setSelectedSeats([]); // Reset seats on tour change
    };

    const handleSeatSelect = (seatId) => {
        setSelectedSeats(prev => {
            if (prev.includes(seatId)) {
                return prev.filter(s => s !== seatId);
            }
            return [...prev, seatId];
        });
    };

    useEffect(() => {
        const newTotal = selectedSeats.reduce((acc, seatId) => acc + SEAT_CONFIG[seatId].price, 0);
        setTotalPrice(newTotal);
    }, [selectedSeats]);
    
    useEffect(() => {
        // If a tour is pre-selected, switch to pilgrimage booking
        if (selectedTour) {
            setBookingType('pilgrimage');
        }
    }, [selectedTour]);

    const handleSubmit = (e) => {
        e.preventDefault();
        // In a real app, you would send this data to a backend
        console.log("Booking Submitted:", {
            bookingType,
            ...formData,
            ...(bookingType === 'pilgrimage' && {
                tour: selectedTour.title,
                selectedSeats,
                totalPrice
            })
        });
        alert("Booking successful! (Check console for details)");
        // Reset form
        setFormData({ name: '', phone: '', email: '', from: '', to: '', date: '' });
        setSelectedSeats([]);
    };

    return (
        <div className="bg-gray-50 min-h-screen py-12 px-4 sm:px-6 lg:px-8">
            <div className="max-w-4xl mx-auto">
                <h1 className="text-center text-4xl font-extrabold text-gray-900 mb-8">Book Your Yatra</h1>

                {/* Booking Type Toggle */}
                <div className="flex justify-center mb-8 bg-amber-100 p-1 rounded-full w-full max-w-md mx-auto">
                    <button
                        onClick={() => setBookingType('point-to-point')}
                        className={`w-1/2 py-2 px-4 rounded-full text-sm font-semibold transition-colors duration-300 ${bookingType === 'point-to-point' ? 'bg-amber-500 text-white shadow' : 'text-amber-800'}`}
                    >
                        Point-to-Point Taxi
                    </button>
                    <button
                        onClick={() => setBookingType('pilgrimage')}
                        className={`w-1/2 py-2 px-4 rounded-full text-sm font-semibold transition-colors duration-300 ${bookingType === 'pilgrimage' ? 'bg-amber-500 text-white shadow' : 'text-amber-800'}`}
                    >
                        Spiritual Pilgrimage
                    </button>
                </div>

                <form onSubmit={handleSubmit} className="bg-white p-8 rounded-2xl shadow-lg space-y-8">
                    {bookingType === 'point-to-point' && (
                        <div className="animate-fade-in">
                            <h2 className="text-2xl font-bold text-gray-800 mb-6 border-l-4 border-amber-500 pl-4">Taxi Booking Details</h2>
                            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                                <div>
                                    <label htmlFor="from" className="block text-sm font-medium text-gray-700">From</label>
                                    <input type="text" name="from" id="from" value={formData.from} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" placeholder="e.g., Udaipur" required />
                                </div>
                                <div>
                                    <label htmlFor="to" className="block text-sm font-medium text-gray-700">To</label>
                                    <input type="text" name="to" id="to" value={formData.to} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" placeholder="e.g., Jodhpur" required />
                                </div>
                                <div className="md:col-span-2">
                                    <label htmlFor="date" className="block text-sm font-medium text-gray-700">Date of Travel</label>
                                    <input type="date" name="date" id="date" value={formData.date} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" required />
                                </div>
                            </div>
                        </div>
                    )}

                    {bookingType === 'pilgrimage' && (
                         <div className="animate-fade-in">
                            <h2 className="text-2xl font-bold text-gray-800 mb-6 border-l-4 border-amber-500 pl-4">Pilgrimage Seat Booking</h2>
                            <div>
                                <label htmlFor="tour-select" className="block text-sm font-medium text-gray-700">Select a Tour</label>
                                <select
                                    id="tour-select"
                                    className="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-amber-500 focus:border-amber-500 sm:text-sm rounded-md"
                                    value={selectedTour ? selectedTour.id : ''}
                                    onChange={handleTourChange}
                                    required
                                >
                                    <option value="" disabled>-- Choose your yatra --</option>
                                    {mockTours.map(tour => (
                                        <option key={tour.id} value={tour.id}>{tour.title} ({formatDate(tour.date)})</option>
                                    ))}
                                </select>
                            </div>

                            {selectedTour && (
                                <div className="mt-8 grid grid-cols-1 md:grid-cols-2 gap-8 items-start">
                                    <div>
                                        <h3 className="font-semibold text-lg text-gray-700 mb-4">Select Your Seat(s)</h3>
                                        <VanLayout 
                                            bookedSeats={selectedTour.bookedSeats}
                                            selectedSeats={selectedSeats}
                                            onSeatSelect={handleSeatSelect}
                                        />
                                        <div className="mt-4 flex flex-wrap gap-4 text-xs">
                                            <div className="flex items-center"><span className="w-4 h-4 rounded bg-amber-100 border-2 border-amber-400 mr-2"></span> Available</div>
                                            <div className="flex items-center"><span className="w-4 h-4 rounded bg-green-500 mr-2"></span> Selected</div>
                                            <div className="flex items-center"><span className="w-4 h-4 rounded bg-gray-400 mr-2"></span> Booked</div>
                                        </div>
                                    </div>
                                    <div className="bg-amber-50 p-6 rounded-lg">
                                        <h3 className="font-semibold text-lg text-gray-800 mb-4">Booking Summary</h3>
                                        <div className="space-y-3">
                                            <p><strong>Tour:</strong> {selectedTour.title}</p>
                                            <p><strong>Date:</strong> {formatDate(selectedTour.date)}</p>
                                            <div>
                                                <strong>Selected Seats:</strong>
                                                {selectedSeats.length > 0 ? (
                                                    <ul className="list-disc list-inside mt-1">
                                                        {selectedSeats.map(s => (
                                                            <li key={s}>Seat {s} (₹{SEAT_CONFIG[s].price})</li>
                                                        ))}
                                                    </ul>
                                                ) : <p className="text-gray-500">No seats selected</p>}
                                            </div>
                                            <hr className="my-3 border-gray-300"/>
                                            <div className="text-xl font-bold text-amber-700">
                                                Total Price: ₹{totalPrice}
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            )}
                        </div>
                    )}

                    {/* Contact Details */}
                    <div className="pt-8 border-t border-gray-200">
                         <h2 className="text-2xl font-bold text-gray-800 mb-6 border-l-4 border-amber-500 pl-4">Your Contact Details</h2>
                         <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label htmlFor="name" className="block text-sm font-medium text-gray-700">Full Name</label>
                                <input type="text" name="name" id="name" value={formData.name} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" required />
                            </div>
                             <div>
                                <label htmlFor="phone" className="block text-sm font-medium text-gray-700">Phone Number</label>
                                <input type="tel" name="phone" id="phone" value={formData.phone} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" required />
                            </div>
                             <div className="md:col-span-2">
                                <label htmlFor="email" className="block text-sm font-medium text-gray-700">Email Address</label>
                                <input type="email" name="email" id="email" value={formData.email} onChange={handleInputChange} className="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:ring-amber-500 focus:border-amber-500" required />
                            </div>
                         </div>
                    </div>
                    
                    <div className="text-center pt-6">
                        <button 
                            type="submit"
                            className="w-full max-w-xs mx-auto bg-amber-600 text-white py-3 px-6 border border-transparent rounded-full shadow-sm text-base font-medium hover:bg-amber-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-amber-500 transition-transform transform hover:scale-105 disabled:bg-gray-400"
                            disabled={bookingType === 'pilgrimage' && (!selectedTour || selectedSeats.length === 0)}
                        >
                            Confirm Booking
                        </button>
                    </div>
                </form>
            </div>
        </div>
    );
};

// About Us Page Component
const AboutPage = () => (
    <div className="bg-white py-16 sm:py-24">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div className="text-center">
                <h2 className="text-base font-semibold text-amber-600 tracking-wide uppercase">About Us</h2>
                <p className="mt-1 text-4xl font-extrabold text-gray-900 sm:text-5xl sm:tracking-tight lg:text-6xl">Shubh Yatra</p>
                <p className="max-w-xl mt-5 mx-auto text-xl text-gray-500">Your trusted partner for spiritual travel.</p>
            </div>
            <div className="mt-12 lg:grid lg:grid-cols-2 lg:gap-8 lg:items-center">
                <div>
                    <h3 className="text-2xl font-extrabold text-gray-900 tracking-tight sm:text-3xl">Our Mission</h3>
                    <p className="mt-3 text-lg text-gray-500">
                        At Shubh Yatra, our mission is to provide seamless, safe, and spiritually enriching travel experiences for pilgrims. We understand the importance of these sacred journeys and are dedicated to managing all travel logistics so you can focus entirely on your devotion and peace of mind.
                    </p>
                    <div className="mt-8 space-y-6">
                        <div className="flex">
                            <div className="flex-shrink-0">
                                <div className="flex items-center justify-center h-12 w-12 rounded-md bg-amber-500 text-white">
                                    {/* Temple Icon */}
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="m2 13 1.4-1.4a2 2 0 0 1 2.82 0L12 17l5.78-5.78a2 2 0 0 1 2.82 0L22 13"/><path d="M6 13H2v6h4v-6Z"/><path d="M18 13h4v6h-4v-6Z"/><path d="M10 3v2"/><path d="M14 3v2"/><path d="M12 17v-5.5a2.5 2.5 0 0 1 5 0V17"/><path d="M12 17v-5.5a2.5 2.5 0 0 0-5 0V17"/></svg>
                                </div>
                            </div>
                            <div className="ml-4">
                                <h4 className="text-lg leading-6 font-medium text-gray-900">Comfort & Safety</h4>
                                <p className="mt-2 text-base text-gray-500">
                                    We prioritize your comfort and safety above all, with well-maintained vehicles and experienced drivers.
                                </p>
                            </div>
                        </div>
                        <div className="flex">
                            <div className="flex-shrink-0">
                                <div className="flex items-center justify-center h-12 w-12 rounded-md bg-amber-500 text-white">
                                    {/* Heart Icon */}
                                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></svg>
                                </div>
                            </div>
                            <div className="ml-4">
                                <h4 className="text-lg leading-6 font-medium text-gray-900">Passionate Service</h4>
                                <p className="mt-2 text-base text-gray-500">
                                    Our team is passionate about spiritual travel and dedicated to making your yatra a memorable one.
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
                <div className="mt-10 lg:mt-0" aria-hidden="true">
                    <img className="mx-auto rounded-lg shadow-xl" src="https://placehold.co/600x450/FDE68A/333333?text=Our+Team" alt="Shubh Yatra Team" />
                </div>
            </div>
        </div>
    </div>
);

// Contact Page Component
const ContactPage = () => (
    <div className="bg-gray-50">
        <div className="max-w-7xl mx-auto py-16 px-4 sm:px-6 lg:px-8">
            <div className="max-w-lg mx-auto md:max-w-none md:grid md:grid-cols-2 md:gap-8">
                <div>
                    <h2 className="text-2xl font-extrabold text-gray-900 sm:text-3xl">Get in touch</h2>
                    <div className="mt-3">
                        <p className="text-lg text-gray-500">
                            Have questions or need to arrange a custom tour? We're here to help. Contact us via phone, email, or visit our office.
                        </p>
                    </div>
                    <div className="mt-9">
                        <div className="flex">
                            <div className="flex-shrink-0">
                                <Phone className="h-6 w-6 text-gray-400" />
                            </div>
                            <div className="ml-3 text-base text-gray-500">
                                <p>+91 12345 67890</p>
                                <p className="mt-1">Mon-Sat, 9am to 7pm</p>
                            </div>
                        </div>
                        <div className="mt-6 flex">
                            <div className="flex-shrink-0">
                                <Mail className="h-6 w-6 text-gray-400" />
                            </div>
                            <div className="ml-3 text-base text-gray-500">
                                <p>contact@shubhyatra.com</p>
                            </div>
                        </div>
                         <div className="mt-6 flex">
                            <div className="flex-shrink-0">
                                <MapPin className="h-6 w-6 text-gray-400" />
                            </div>
                            <div className="ml-3 text-base text-gray-500">
                                <p>123 Yatra Marg, Udaipur, Rajasthan</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div className="mt-12 sm:mt-16 md:mt-0">
                    <h2 className="text-2xl font-extrabold text-gray-900 sm:text-3xl">Send us a message</h2>
                    <form action="#" method="POST" className="mt-9 grid grid-cols-1 gap-y-6 sm:grid-cols-2 sm:gap-x-8">
                        <div>
                            <label htmlFor="first-name" className="block text-sm font-medium text-gray-700">First name</label>
                            <div className="mt-1">
                                <input type="text" name="first-name" id="first-name" autoComplete="given-name" className="block w-full shadow-sm sm:text-sm focus:ring-amber-500 focus:border-amber-500 border-gray-300 rounded-md" />
                            </div>
                        </div>
                        <div>
                            <label htmlFor="last-name" className="block text-sm font-medium text-gray-700">Last name</label>
                            <div className="mt-1">
                                <input type="text" name="last-name" id="last-name" autoComplete="family-name" className="block w-full shadow-sm sm:text-sm focus:ring-amber-500 focus:border-amber-500 border-gray-300 rounded-md" />
                            </div>
                        </div>
                        <div className="sm:col-span-2">
                            <label htmlFor="email" className="block text-sm font-medium text-gray-700">Email</label>
                            <div className="mt-1">
                                <input id="email" name="email" type="email" autoComplete="email" className="block w-full shadow-sm sm:text-sm focus:ring-amber-500 focus:border-amber-500 border-gray-300 rounded-md" />
                            </div>
                        </div>
                        <div className="sm:col-span-2">
                            <label htmlFor="message" className="block text-sm font-medium text-gray-700">Message</label>
                            <div className="mt-1">
                                <textarea id="message" name="message" rows="4" className="block w-full shadow-sm sm:text-sm focus:ring-amber-500 focus:border-amber-500 border border-gray-300 rounded-md"></textarea>
                            </div>
                        </div>
                        <div className="text-right sm:col-span-2">
                            <button type="submit" className="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-amber-600 hover:bg-amber-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-amber-500">
                                Submit
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
);

// Footer Component
const Footer = ({ setPage }) => (
    <footer className="bg-gray-800 text-white">
        <div className="max-w-7xl mx-auto py-12 px-4 sm:px-6 lg:px-8">
            <div className="xl:grid xl:grid-cols-3 xl:gap-8">
                <div className="space-y-8 xl:col-span-1">
                    <h2 className="text-3xl font-bold text-amber-400">Shubh Yatra</h2>
                    <p className="text-gray-300 text-base">
                        Facilitating blessed journeys to India's most sacred destinations with comfort and care.
                    </p>
                </div>
                <div className="mt-12 grid grid-cols-2 gap-8 xl:mt-0 xl:col-span-2">
                    <div className="md:grid md:grid-cols-2 md:gap-8">
                        <div>
                            <h3 className="text-sm font-semibold text-gray-200 tracking-wider uppercase">Quick Links</h3>
                            <ul className="mt-4 space-y-4">
                                <li><a href="#" onClick={() => setPage('about')} className="text-base text-gray-300 hover:text-white">About</a></li>
                                <li><a href="#" onClick={() => setPage('tours')} className="text-base text-gray-300 hover:text-white">Tours</a></li>
                                <li><a href="#" onClick={() => setPage('booking')} className="text-base text-gray-300 hover:text-white">Booking</a></li>
                            </ul>
                        </div>
                        <div className="mt-12 md:mt-0">
                            <h3 className="text-sm font-semibold text-gray-200 tracking-wider uppercase">Contact</h3>
                            <ul className="mt-4 space-y-4">
                                <li><a href="#" onClick={() => setPage('contact')} className="text-base text-gray-300 hover:text-white">Contact Us</a></li>
                                <li><p className="text-base text-gray-300">FAQs</p></li>
                            </ul>
                        </div>
                    </div>
                    <div className="md:grid md:grid-cols-1 md:gap-8">
                        <div>
                            <h3 className="text-sm font-semibold text-gray-200 tracking-wider uppercase">Legal</h3>
                            <ul className="mt-4 space-y-4">
                                <li><p className="text-base text-gray-300">Privacy Policy</p></li>
                                <li><p className="text-base text-gray-300">Terms of Service</p></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div className="mt-12 border-t border-gray-700 pt-8">
                <p className="text-base text-gray-400 text-center">&copy; {new Date().getFullYear()} Shubh Yatra. All rights reserved.</p>
            </div>
        </div>
    </footer>
);

// Main App Component
export default function App() {
    const [page, setPage] = useState('home');
    const [selectedTour, setSelectedTour] = useState(null);

    const renderPage = () => {
        switch (page) {
            case 'home':
                return <HomePage setPage={setPage} setSelectedTour={setSelectedTour} />;
            case 'tours':
                return <ToursPage setPage={setPage} setSelectedTour={setSelectedTour} />;
            case 'booking':
                return <BookingPage selectedTour={selectedTour} setSelectedTour={setSelectedTour} />;
            case 'about':
                return <AboutPage />;
            case 'contact':
                return <ContactPage />;
            default:
                return <HomePage setPage={setPage} setSelectedTour={setSelectedTour} />;
        }
    };

    return (
        <div className="bg-gray-50 font-sans">
            <Navbar setPage={setPage} />
            <main>
                {renderPage()}
            </main>
            <Footer setPage={setPage} />
        </div>
    );
}
