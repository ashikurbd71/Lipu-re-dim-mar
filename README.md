# 🥚 Egg Burst Game

A fun and interactive egg bursting game built with Vue.js, featuring dramatic burst effects, sound, and mobile responsiveness.

## 🎮 Features

- **Clickable Eggs**: Click on eggs to burst them and see the funny image
- **Dramatic Burst Effects**: Multiple visual effects including particles, rings, and flash
- **Sound Effects**: Strong smash sound plays on impact
- **15-Second Display**: Images stay visible for 15 seconds after bursting
- **Mobile Responsive**: Optimized for both desktop and mobile devices
- **Score System**: Track your points as you burst eggs
- **Beautiful UI**: Modern gradient background with glassmorphism effects

## 🚀 Live Demo

[Play the Game](https://your-deployment-url.com)

## 🛠️ Technologies Used

- **Vue.js 3** - Progressive JavaScript framework
- **Vite** - Fast build tool and dev server
- **CSS3** - Modern styling with animations
- **HTML5 Audio** - Sound effects
- **Responsive Design** - Mobile-first approach

## 📱 Mobile Responsive Features

- Touch-optimized controls
- Responsive layout for all screen sizes
- Optimized button sizes for mobile
- Proper touch handling
- Mobile-friendly animations

## 🎯 How to Play

1. **Click on eggs** to burst them
2. **Watch the dramatic burst effect** with particles and animations
3. **Hear the smash sound** and see screen shake
4. **View the funny image** for 15 seconds
5. **Score points** for each egg you burst
6. **Add more eggs** with the "Add Egg" button
7. **Reset the game** anytime with the "Reset Game" button

## 🚀 Deployment

### Option 1: Deploy to Netlify

1. **Build the project**:

   ```bash
   npm run build
   ```

2. **Deploy to Netlify**:
   - Go to [Netlify](https://netlify.com)
   - Drag and drop the `dist` folder
   - Or connect your GitHub repository for automatic deployments

### Option 2: Deploy to Vercel

1. **Install Vercel CLI**:

   ```bash
   npm i -g vercel
   ```

2. **Deploy**:
   ```bash
   vercel
   ```

### Option 3: Deploy to GitHub Pages

1. **Add deploy script** to `package.json`:

   ```json
   {
     "scripts": {
       "deploy": "gh-pages -d dist"
     }
   }
   ```

2. **Install gh-pages**:

   ```bash
   npm install --save-dev gh-pages
   ```

3. **Build and deploy**:
   ```bash
   npm run build
   npm run deploy
   ```

## 🛠️ Local Development

1. **Clone the repository**:

   ```bash
   git clone https://github.com/your-username/lipu-egg-game.git
   cd lipu-egg-game
   ```

2. **Install dependencies**:

   ```bash
   npm install
   ```

3. **Start development server**:

   ```bash
   npm run dev
   ```

4. **Build for production**:
   ```bash
   npm run build
   ```

## 📁 Project Structure

```
lipu-egg-game/
├── public/
│   ├── sound.mp3          # Sound effect file
│   ├── funnyumage.jpg     # Funny image for burst effect
│   └── vite.svg           # Vite logo
├── src/
│   ├── App.vue            # Main game component
│   ├── main.js            # Vue app entry point
│   ├── style.css          # Global styles
│   └── assets/            # Static assets
├── index.html             # HTML template
├── package.json           # Dependencies and scripts
└── README.md              # This file
```

## 🎨 Customization

### Changing the Sound

Replace `public/sound.mp3` with your own sound file.

### Changing the Image

Replace `public/funnyumage.jpg` with your own image.

### Modifying Colors

Edit the CSS variables in `src/App.vue` to change the color scheme.

### Adjusting Timing

Modify the animation durations in the CSS animations.

## 📱 Mobile Optimization

The game is fully optimized for mobile devices with:

- Touch-friendly controls
- Responsive design
- Optimized performance
- Mobile-specific animations

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👨‍💻 Developer

**Developed by [Ashikur Rahman Ovi](https://github.com/ashikurbd71)**

- GitHub: [@ashikurbd71](https://github.com/ashikurbd71)
- Portfolio: [ashikurovi.netlify.app](https://ashikurovi.netlify.app/)
- LinkedIn: [ashikur-rahman-ovi](https://linkedin.com/in/ashikur-rahman-ovi)

## 🙏 Acknowledgments

- Vue.js team for the amazing framework
- Vite team for the fast build tool
- All contributors and testers

---

⭐ **Star this repository if you found it helpful!**
