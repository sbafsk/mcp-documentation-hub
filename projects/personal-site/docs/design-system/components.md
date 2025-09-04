# Design System & Component Guidelines

> **AI Context**: This is the single source of truth for UI/UX design and component usage.
> For current status: see `../status/progress.yaml`
> For system overview: see `../architecture/overview.md`

## Design System Overview

Sebastián's Personal Site uses shadcn/ui components with a consistent design language focused on professionalism, readability, and modern aesthetics.

## Color Palette

### Primary Colors
- **Primary**: Blue-600 (#2563eb) - Professional trust
- **Secondary**: Gray-700 (#374151) - Text and borders
- **Accent**: Emerald-500 (#10b981) - Success and highlights

### Semantic Colors
- **Success**: Green-500 (#22c55e)
- **Warning**: Yellow-500 (#eab308)
- **Error**: Red-500 (#ef4444)
- **Info**: Blue-500 (#3b82f6)

## Typography

### Font Hierarchy
- **Heading 1**: Inter, 48px, font-weight 700 (Hero title)
- **Heading 2**: Inter, 36px, font-weight 600 (Section titles)
- **Heading 3**: Inter, 24px, font-weight 600 (Subsection titles)
- **Body**: Inter, 16px, font-weight 400 (Main text)
- **Small**: Inter, 14px, font-weight 400 (Captions, metadata)

### Font Stack
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

## Component Usage by Section

### 1. Hero Section

#### Recommended Components
- **Card** (`<Card>`): Main hero container with gradient background
- **Button** (`<Button>`): Primary CTA buttons (Download CV, Contact)
- **Badge** (`<Badge>`): Professional title and location tags

#### Implementation Example
```tsx
import { Card, CardContent } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"

const HeroSection = () => {
  return (
    <Card className="hero-card bg-gradient-to-br from-blue-50 to-indigo-100">
      <CardContent className="text-center py-20">
        <h1 className="text-5xl font-bold text-gray-900 mb-4">
          Sebastián Pereira Rivero
        </h1>
        <Badge variant="secondary" className="text-lg px-4 py-2 mb-6">
          Senior Frontend Developer
        </Badge>
        <div className="flex gap-4 justify-center">
          <Button size="lg">Download CV</Button>
          <Button variant="outline" size="lg">Contact Me</Button>
        </div>
      </CardContent>
    </Card>
  )
}
```

### 2. About/Profile Section

#### Recommended Components
- **Card** (`<Card>`): Profile information container
- **Avatar** (`<Avatar>`): Professional photo
- **Separator** (`<Separator>`): Visual section dividers

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import { Separator } from "@/components/ui/separator"

const AboutSection = () => {
  return (
    <Card>
      <CardHeader>
        <CardTitle>About Me</CardTitle>
      </CardHeader>
      <CardContent className="flex gap-6">
        <Avatar className="w-24 h-24">
          <AvatarImage src="/profile-photo.jpg" />
          <AvatarFallback>SP</AvatarFallback>
        </Avatar>
        <div className="flex-1">
          <p className="text-gray-700 leading-relaxed">
            Frontend Developer with 7+ years of experience building and scaling modern web applications...
          </p>
          <Separator className="my-4" />
          <div className="flex gap-2">
            <Badge variant="outline">React</Badge>
            <Badge variant="outline">Next.js</Badge>
            <Badge variant="outline">TypeScript</Badge>
          </div>
        </div>
      </CardContent>
    </Card>
  )
}
```

### 3. Experience Timeline

#### Recommended Components
- **Card** (`<Card>`): Individual experience items
- **Badge** (`<Badge>`): Technology tags and company names
- **Separator** (`<Separator>`): Timeline visual elements

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Separator } from "@/components/ui/separator"

const ExperienceSection = () => {
  return (
    <div className="space-y-6">
      <h2 className="text-3xl font-semibold">Experience</h2>
      {experiences.map((exp, index) => (
        <Card key={index} className="relative">
          <CardHeader>
            <div className="flex justify-between items-start">
              <div>
                <CardTitle className="text-xl">{exp.title}</CardTitle>
                <p className="text-gray-600">{exp.company}</p>
              </div>
              <Badge variant="secondary">{exp.period}</Badge>
            </div>
          </CardHeader>
          <CardContent>
            <p className="text-gray-700 mb-4">{exp.description}</p>
            <div className="flex flex-wrap gap-2">
              {exp.technologies.map((tech) => (
                <Badge key={tech} variant="outline">{tech}</Badge>
              ))}
            </div>
          </CardContent>
          {index < experiences.length - 1 && (
            <Separator className="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-px h-8 bg-gray-300" />
          )}
        </Card>
      ))}
    </div>
  )
}
```

### 4. Skills Showcase

#### Recommended Components
- **Card** (`<Card>`): Skill category containers
- **Progress** (`<Progress>`): Skill proficiency indicators
- **Badge** (`<Badge>`): Skill names and categories

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Progress } from "@/components/ui/progress"
import { Badge } from "@/components/ui/badge"

const SkillsSection = () => {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {skillCategories.map((category) => (
        <Card key={category.name}>
          <CardHeader>
            <CardTitle className="text-lg">{category.name}</CardTitle>
          </CardHeader>
          <CardContent className="space-y-4">
            {category.skills.map((skill) => (
              <div key={skill.name}>
                <div className="flex justify-between items-center mb-2">
                  <span className="text-sm font-medium">{skill.name}</span>
                  <Badge variant="secondary">{skill.level}</Badge>
                </div>
                <Progress value={skill.proficiency} className="h-2" />
              </div>
            ))}
          </CardContent>
        </Card>
      ))}
    </div>
  )
}
```

### 5. Project Gallery

#### Recommended Components
- **Card** (`<Card>`): Project showcase containers
- **Button** (`<Button>`): Project links and actions
- **Badge** (`<Badge>`): Project technologies and status
- **AspectRatio** (`<AspectRatio>`): Project image containers

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"
import { AspectRatio } from "@/components/ui/aspect-ratio"

const ProjectsSection = () => {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {projects.map((project) => (
        <Card key={project.id} className="overflow-hidden">
          <AspectRatio ratio={16 / 9}>
            <img 
              src={project.image} 
              alt={project.title}
              className="object-cover w-full h-full"
            />
          </AspectRatio>
          <CardHeader>
            <CardTitle className="text-lg">{project.title}</CardTitle>
            <p className="text-sm text-gray-600">{project.description}</p>
          </CardHeader>
          <CardContent>
            <div className="flex flex-wrap gap-2 mb-4">
              {project.technologies.map((tech) => (
                <Badge key={tech} variant="outline">{tech}</Badge>
              ))}
            </div>
            <div className="flex gap-2">
              <Button size="sm" variant="outline">View Demo</Button>
              <Button size="sm" variant="outline">Source Code</Button>
            </div>
          </CardContent>
        </Card>
      ))}
    </div>
  )
}
```

### 6. Contact Form

#### Recommended Components
- **Card** (`<Card>`): Form container
- **Input** (`<Input>`): Text input fields
- **Textarea** (`<Textarea>`): Message field
- **Button** (`<Button>`): Submit button
- **Label** (`<Label>`): Form field labels
- **Alert** (`<Alert>`): Success/error messages

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Textarea } from "@/components/ui/textarea"
import { Button } from "@/components/ui/button"
import { Label } from "@/components/ui/label"
import { Alert, AlertDescription } from "@/components/ui/alert"

const ContactForm = () => {
  return (
    <Card className="max-w-2xl mx-auto">
      <CardHeader>
        <CardTitle>Get In Touch</CardTitle>
        <p className="text-gray-600">
          I'm always interested in new opportunities and collaborations.
        </p>
      </CardHeader>
      <CardContent>
        <form className="space-y-4">
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <Label htmlFor="name">Name</Label>
              <Input id="name" placeholder="Your name" required />
            </div>
            <div>
              <Label htmlFor="email">Email</Label>
              <Input id="email" type="email" placeholder="your@email.com" required />
            </div>
          </div>
          <div>
            <Label htmlFor="subject">Subject</Label>
            <Input id="subject" placeholder="What's this about?" required />
          </div>
          <div>
            <Label htmlFor="message">Message</Label>
            <Textarea 
              id="message" 
              placeholder="Tell me more about your project or opportunity..."
              rows={5}
              required 
            />
          </div>
          <Button type="submit" className="w-full">
            Send Message
          </Button>
        </form>
        
        {success && (
          <Alert className="mt-4">
            <AlertDescription>
              Thank you! Your message has been sent successfully.
            </AlertDescription>
          </Alert>
        )}
      </CardContent>
    </Card>
  )
}
```

### 7. CV Download Section

#### Recommended Components
- **Card** (`<Card>`): Download container
- **Button** (`<Button>`): Download buttons with icons
- **Badge** (`<Badge>`): File format indicators

#### Implementation Example
```tsx
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"
import { Download, FileText, FilePdf } from "lucide-react"

const CVDownloadSection = () => {
  return (
    <Card className="text-center">
      <CardHeader>
        <CardTitle>Download My CV</CardTitle>
        <p className="text-gray-600">
          Get a copy of my professional resume in your preferred format
        </p>
      </CardHeader>
      <CardContent className="space-y-4">
        <div className="flex justify-center gap-4">
          <Button size="lg" className="flex items-center gap-2">
            <FilePdf className="w-5 h-5" />
            PDF Version
          </Button>
          <Button variant="outline" size="lg" className="flex items-center gap-2">
            <FileText className="w-5 h-5" />
            Word Version
          </Button>
        </div>
        <div className="flex justify-center gap-2">
          <Badge variant="secondary">PDF</Badge>
          <Badge variant="secondary">DOCX</Badge>
          <Badge variant="secondary">Updated: Dec 2024</Badge>
        </div>
      </CardContent>
    </Card>
  )
}
```

## Layout Components

### Navigation Header
```tsx
import { Button } from "@/components/ui/button"
import { Sheet, SheetContent, SheetTrigger } from "@/components/ui/sheet"
import { Menu } from "lucide-react"

const Header = () => {
  return (
    <header className="sticky top-0 z-50 w-full border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
      <div className="container flex h-16 items-center">
        <div className="mr-4 hidden md:flex">
          <nav className="flex items-center space-x-6 text-sm font-medium">
            <a href="#about" className="transition-colors hover:text-foreground/80">About</a>
            <a href="#experience" className="transition-colors hover:text-foreground/80">Experience</a>
            <a href="#projects" className="transition-colors hover:text-foreground/80">Projects</a>
            <a href="#contact" className="transition-colors hover:text-foreground/80">Contact</a>
          </nav>
        </div>
        <Sheet>
          <SheetTrigger asChild>
            <Button variant="ghost" className="md:hidden">
              <Menu className="h-5 w-5" />
            </Button>
          </SheetTrigger>
          <SheetContent side="left">
            <nav className="flex flex-col space-y-4">
              <a href="#about">About</a>
              <a href="#experience">Experience</a>
              <a href="#projects">Projects</a>
              <a href="#contact">Contact</a>
            </nav>
          </SheetContent>
        </Sheet>
      </div>
    </header>
  )
}
```

## Responsive Design Guidelines

### Breakpoints
- **Mobile**: < 768px (default)
- **Tablet**: 768px - 1024px
- **Desktop**: > 1024px

### Mobile-First Approach
- Start with mobile layout
- Use `md:` and `lg:` prefixes for larger screens
- Ensure touch-friendly button sizes (min 44px)
- Optimize spacing for mobile viewing

## Accessibility Guidelines

### ARIA Labels
- Use semantic HTML elements
- Add descriptive labels for interactive elements
- Ensure proper heading hierarchy
- Provide alt text for images

### Keyboard Navigation
- All interactive elements must be keyboard accessible
- Visible focus indicators
- Logical tab order
- Skip links for main content

## Animation Guidelines

### Transitions
- **Duration**: 200ms for micro-interactions, 300ms for page transitions
- **Easing**: `ease-in-out` for smooth, professional feel
- **Hover Effects**: Subtle scale and shadow changes
- **Page Transitions**: Fade in/out with staggered content

### Performance
- Use CSS transforms and opacity for animations
- Avoid animating layout properties
- Use `will-change` sparingly
- Test on lower-end devices

## Related Documentation

- [System Architecture](../architecture/overview.md)
- [Component Architecture](architecture.md)
- [Current Status](../status/progress.yaml)
- [Setup Guide](../guides/setup.md)

