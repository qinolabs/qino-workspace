Pixel-perfect recreation of the existing Gatsby + Strapi portfolio as a static TanStack Start app on Cloudflare Workers. No CMS, no backend — content lives as TypeScript data modules, images in R2.

The original architecture: Strapi 4.15.4 (PostgreSQL, S3, REST API) powering a Gatsby 5 frontend with D3 Voronoi visualization, Tailwind styling, dark mode, and responsive layouts across 4 pages. The new architecture: a single static frontend app using `template-frontend-static`, bundled content, and R2 for media.

Three deep-dive indexes created: Strapi content model extraction guide, Gatsby component/styling reproduction map, and the static app migration strategy with phased implementation plan.