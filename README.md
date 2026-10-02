const counters = document.querySelectorAll('[data-target]');

const formatValue = (value) => {
  if (value >= 1000) {
    return `${(value / 1000).toFixed(1)}k`;
  }
  return value.toString();
};

const animateCounter = (element) => {
  const target = Number(element.dataset.target || 0);
  const duration = 1200;
  const start = performance.now();

  const tick = (now) => {
    const progress = Math.min((now - start) / duration, 1);
    const eased = 1 - (1 - progress) ** 3;
    const value = Math.round(target * eased);
    element.textContent = formatValue(value);

    if (progress < 1) {
      requestAnimationFrame(tick);
    }
  };

  requestAnimationFrame(tick);
};

counters.forEach(animateCounter);

const liveTimestamp = document.createElement('div');
liveTimestamp.className = 'live-timestamp';
liveTimestamp.textContent = 'Updated just now';
liveTimestamp.style.cssText = `
  position: fixed;
  right: 22px;
  bottom: 18px;
  background: rgba(12, 29, 42, 0.88);
  border: 1px solid rgba(136, 181, 255, 0.2);
  color: #8aa6c4;
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  border-radius: 999px;
  padding: 8px 12px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.2);
`;

document.body.appendChild(liveTimestamp);

const updateTimestamp = () => {
  const now = new Date();
  const time = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
  liveTimestamp.textContent = `Updated ${time}`;
};

setInterval(updateTimestamp, 30000);
