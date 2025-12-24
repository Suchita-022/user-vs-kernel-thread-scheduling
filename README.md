# Online Classroom Thread Scheduler - Next.js Dashboard

## Project Overview

This project compares **user-level thread scheduling** with **kernel-level thread scheduling** in the context of handling thousands of student queries in an online classroom environment. It includes a modern Next.js dashboard for real-time performance visualization and detailed technical analysis.

## Features

### Interactive Dashboard
- **Live Simulation Controls**: Configure thread count (1-32) and query volume (100-10,000)
- **Real-time Performance Metrics**: Total time, wait time, execution time, throughput
- **Visual Comparisons**: Charts showing queue evolution and execution time distribution
- **Technical Details Panel**: System characteristics and performance indicators
- **Responsive Design**: Works seamlessly on desktop and mobile devices

### Performance Analysis
- Side-by-side comparison table with percentage differences
- Pie chart visualization of total execution time
- Timeline chart showing queue evolution during simulation
- Thread utilization metrics and context switch counting

### Educational Resources
- Detailed technical report (REPORT.md)
- Implementation comparisons
- Recommendations for production systems

## Project Structure

```
├── app/
│   ├── layout.tsx                          # Root layout with metadata
│   ├── page.tsx                            # Home page
│   ├── globals.css                         # Styling and design tokens
│   └── api/
│       └── simulate/
│           └── route.ts                    # Simulation API endpoint
├── components/
│   └── thread-scheduler-dashboard.tsx      # Main dashboard component
├── backend/                                 # C backend files (reference)
│   ├── thread_pool.h
│   ├── thread_pool.c
│   ├── user_level_threads.c
│   ├── kernel_level_threads.c
│   └── server.c
├── frontend/                                # Legacy HTML frontend (reference)
│   ├── index.html
│   ├── styles.css
│   └── script.js
├── README.md                               # This file
└── REPORT.md                               # Technical report
```

## Getting Started

### Prerequisites
- Node.js 18+ and npm/pnpm
- Modern web browser

### Installation

```bash
# Install dependencies
npm install
# or
pnpm install
```

### Running the Development Server

```bash
npm run dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to see the dashboard.

### Building for Production

```bash
npm run build
npm run start
```

## How to Use the Dashboard

1. **Configure Simulation**: Set the number of threads (1-32) and student queries (100-10,000)
2. **Run Simulation**: Click the "Run Simulation" button to start the comparison
3. **View Results**: Analyze performance metrics across multiple comparison modes:
   - **Metrics Table**: Detailed side-by-side comparison with percentage differences
   - **Performance Charts**: Visual representation of queue evolution and execution times
   - **Technical Details**: In-depth system characteristics and utilization metrics

## Key Concepts

### User-Level Threading
- **Scheduler**: Application manages thread scheduling in user space
- **Speed**: Very fast context switching (no OS kernel involvement)
- **Parallelism**: Limited to single processor (no true multicore parallelism)
- **Use Case**: Simple systems, real-time applications with strict latency requirements
- **Limitation**: Cannot utilize multiple CPU cores effectively

### Kernel-Level Threading
- **Scheduler**: OS kernel manages thread scheduling and execution
- **Speed**: Higher overhead due to system calls
- **Parallelism**: True parallelism on multicore systems
- **Use Case**: Production systems, I/O-intensive applications, multicore optimization
- **Advantage**: Automatic load balancing and preemption

## Performance Results

### Typical Metrics (1000 queries, 4 threads)
| Metric | User-Level | Kernel-Level | Winner |
|--------|-----------|-------------|--------|
| Total Time | ~450ms | ~275ms | Kernel (1.6x faster) |
| Avg Wait Time | ~15ms | ~8ms | Kernel |
| Throughput | ~2,222 q/s | ~3,636 q/s | Kernel |
| Context Switches | ~250 | ~120 | Kernel (fewer) |

### Scalability
- **User-Level**: Flat performance regardless of thread count (single-core limited)
- **Kernel-Level**: Near-linear improvement with additional cores

## API Reference

### POST /api/simulate

Runs a simulation comparing both threading models.

**Request Body:**
```json
{
  "numThreads": 4,
  "numQueries": 1000
}
```

**Response:**
```json
{
  "userLevel": {
    "totalTime": 450.25,
    "waitTime": 15.3,
    "executionTime": 10.2,
    "throughput": 2222.5,
    "threadUtilization": 85.5,
    "contextSwitches": 250
  },
  "kernelLevel": {
    "totalTime": 275.50,
    "waitTime": 8.1,
    "executionTime": 10.2,
    "throughput": 3636.4,
    "threadUtilization": 92.3,
    "contextSwitches": 120
  },
  "timeline": [
    { "time": 0, "userLevelQueue": 1000, "kernelLevelQueue": 1000 },
    { "time": 50, "userLevelQueue": 950, "kernelLevelQueue": 920 },
    ...
  ]
}
```

## Recommendations

### Use User-Level Threading When:
- Simple scheduling requirements
- Single-core systems
- Minimal context switching overhead is critical
- Embedded or IoT applications
- Real-time constraints are tight

### Use Kernel-Level Threading When:
- Multicore systems available
- Large-scale applications (100+ concurrent users)
- Production online classroom platforms
- Complex synchronization needs
- Scalability is important

## Technical Implementation

### Simulation Algorithm
The Next.js API route simulates both threading models:

1. **User-Level**: Round-robin scheduling with minimal overhead
2. **Kernel-Level**: Optimal load balancing with preemption simulation

Both simulations track:
- Queue wait times
- Execution times per query
- Thread utilization percentages
- Context switch frequency
- Timeline evolution

### Stack
- **Frontend**: Next.js 16, React 19, TypeScript
- **UI Components**: shadcn/ui, Recharts for visualization
- **Styling**: Tailwind CSS 4 with custom design tokens
- **Charts**: Recharts for interactive performance visualization

## Future Enhancements

1. **Priority Scheduling**: Implement priority-based query handling
2. **Real Backend**: Integrate C backend via child processes
3. **Database Integration**: Store simulation results
4. **Advanced Metrics**: CPU and memory profiling
5. **Distributed Scheduling**: Multi-machine thread coordination
6. **ML Predictions**: Predict optimal thread count

## Educational Value

This project demonstrates:
1. Operating system thread scheduling concepts
2. Producer-consumer problem and queue management
3. Performance measurement and comparative analysis
4. System-level programming principles
5. Modern full-stack web development (Next.js)
6. Interactive data visualization
7. Real-world application architecture

## References

- POSIX Threads Programming Guide
- Linux Kernel CFS Scheduler Documentation
- "Modern Operating Systems" by Andrew S. Tanenbaum
- "The Linux Programming Interface" by Michael Kerrisk
- Next.js Documentation: https://nextjs.org/docs
- Recharts Documentation: https://recharts.org

## License

Educational project for learning and demonstration purposes.

---

**Last Updated**: December 2024
**Status**: Production-Ready
**Technology**: Next.js 16, React 19, TypeScript, Tailwind CSS
